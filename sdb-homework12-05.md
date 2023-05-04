 # Домашнее задание к занятию «Индексы» - `Коробов Евгений`

### Задание 1.
Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.
### Ответ
```
SELECT (SUM(INDEX_LENGTH) / SUM(DATA_LENGTH + INDEX_LENGTH)) * 100 AS index_to_table_ratio
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'sakila';
```
 
## Задание 2. 
Выполните explain analyze следующего запроса:
```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
перечислите узкие места; оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

### Ответ

Узкие места:
### - Seq Scan на таблице payment: 16049 строк удаляются по фильтру date(payment_date) = '2005-07-30'::date, что может быть заменено на использование индекса по полю payment_date.
### - Hash Join на таблицах rental и customer: здесь можно добавить индекс на поле customer_id в обеих таблицах, что должно ускорить соединение.
### - Hash Join на таблицах rental и inventory: здесь также можно добавить индекс на поле inventory_id в обеих таблицах, что должно ускорить соединение.

Оптимизированный запрос:
```
SELECT DISTINCT CONCAT(c.last_name, ' ', c.first_name), SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title)
FROM payment p
JOIN rental r ON r.rental_id = p.rental_id 
JOIN customer c ON c.customer_id = r.customer_id 
JOIN inventory i ON i.inventory_id = r.inventory_id 
JOIN film f ON f.film_id = i.film_id 
WHERE p.payment_date = '2005-07-30' 
AND r.rental_date = p.payment_date 
AND c.customer_id IN (
    SELECT r.customer_id 
    FROM rental r 
    WHERE r.rental_date = p.payment_date
) 
AND i.inventory_id IN (
    SELECT r.inventory_id 
    FROM rental r 
    WHERE r.rental_date = p.payment_date
);
CREATE INDEX payment_payment_date_idx ON payment(payment_date);
CREATE INDEX rental_customer_id_idx ON rental(customer_id);
CREATE INDEX rental_inventory_id_idx ON rental(inventory_id);
CREATE INDEX inventory_film_id_idx ON inventory(film_id);
```
