 # Домашнее задание к занятию «SQL. Часть 2» - `Коробов Евгений`

### Задание 1.
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.
### Ответ
```
SELECT employees.last_name, employees.first_name, stores.city, COUNT(users.id) as users_count
FROM stores
JOIN employees ON employees.store_id = stores.id
JOIN users ON users.store_id = stores.id
WHERE stores.id IN (
    SELECT store_id
    FROM users
    GROUP BY store_id
    HAVING COUNT(*) > 300
)
GROUP BY stores.id
HAVING COUNT(users.id) > 300;
```
 
## Задание 2. 
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
### Ответ
```
SELECT COUNT(*) AS count_longer_than_average
FROM film
WHERE length > (
    SELECT AVG(length)
    FROM film
);
```
## Задание 3. 
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
### Ответ
```
SELECT 
    DATE_TRUNC('month', rental.rental_date) AS month, 
    COUNT(DISTINCT rental.rental_id) AS rentals_count, 
    SUM(payment.amount) AS total_payments
FROM rental
LEFT JOIN payment ON payment.rental_id = rental.rental_id
GROUP BY month
ORDER BY total_payments DESC
LIMIT 1;
```
