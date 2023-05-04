 # Домашнее задание к занятию «SQL. Часть 2» - `Коробов Евгений`

### Задание 1.
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.
### Ответ
```
SELECT s.first_name, s.last_name, a.city, COUNT(*) AS customer_count
FROM store st
JOIN staff s ON st.store_id = s.store_id
JOIN customer c ON c.store_id = st.store_id
JOIN address a ON st.address_id = a.address_id
GROUP BY s.first_name, s.last_name, a.city
HAVING COUNT(*) > 300;
```
 
## Задание 2. 
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
### Ответ
```
SELECT COUNT(*) AS film_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
## Задание 3. 
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
### Ответ
```
SELECT DATE_FORMAT(p.payment_date, '%M %Y') AS month, COUNT(r.rental_id) AS rental_count
FROM payment p
JOIN rental r ON p.rental_id = r.rental_id
GROUP BY month
ORDER BY SUM(p.amount) DESC
LIMIT 1;
```
