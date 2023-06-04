# «SQL. Часть 2»
# Александр Широбоков
## Задание 1
### Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:
 - фамилия и имя сотрудника из этого магазина;
 - город нахождения магазина;
 - количество пользователей, закреплённых в этом магазине.

![Снимок экрана (227)](https://github.com/AleksandrShirobokov/SQL.-2/assets/69298696/8b3165ce-f001-401d-a986-04b0e34a9bb1)
```
SELECT
  staff.first_name,
  staff.last_name,
  city.city,
  COUNT(customer.customer_id) AS customer_count
FROM
  staff
  JOIN store ON staff.store_id = store.store_id
  JOIN address ON store.address_id = address.address_id
  JOIN city ON address.city_id = city.city_id
  JOIN customer ON staff.staff_id = customer.store_id
GROUP BY
  staff.staff_id
HAVING
  COUNT(customer.customer_id) > 300;
```
## Задание 2
### Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
![Снимок экрана (228)](https://github.com/AleksandrShirobokov/SQL.-2/assets/69298696/51e175a8-4e78-413e-8b24-04feead189a8)
```
SELECT COUNT(*) AS film_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
## Задание 3
### Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
![Снимок экрана (229)](https://github.com/AleksandrShirobokov/SQL.-2/assets/69298696/5a3b2f08-d9dd-44b0-a8cf-b0c4911fb5e9)
```
SELECT
  MONTH(payment_date) AS month,
  SUM(amount) AS total_payment_amount,
  COUNT(rental_id) AS rental_count
FROM
  payment
GROUP BY
  MONTH(payment_date)
ORDER BY
  total_payment_amount DESC
LIMIT 1;
```
## Задание 4
### Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».
![Снимок экрана (230)](https://github.com/AleksandrShirobokov/SQL.-2/assets/69298696/6d6e3709-2a42-438e-ab2a-2a734d5fa18b)
```
SELECT
  staff.staff_id,
  CONCAT(staff.first_name, ' ', staff.last_name) AS full_name,
  COUNT(*) AS sales_count,
  CASE WHEN COUNT(*) > 8000 THEN 'Да' ELSE 'Нет' END AS Премия
FROM
  payment
  JOIN staff ON payment.staff_id = staff.staff_id
GROUP BY
  staff.staff_id;
```
## Задание 5
### Найдите фильмы, которые ни разу не брали в аренду.
![Снимок экрана (231)](https://github.com/AleksandrShirobokov/SQL.-2/assets/69298696/c3053328-4887-458f-9ff3-d152eb139d42)
```
SELECT film.film_id, film.title
FROM film
LEFT JOIN rental ON film.film_id = rental.inventory_id
WHERE rental.inventory_id IS NULL;
```
### Общий код:
```
/*Задание 1*/
SELECT
  staff.first_name,
  staff.last_name,
  city.city,
  COUNT(customer.customer_id) AS customer_count
FROM
  staff
  JOIN store ON staff.store_id = store.store_id
  JOIN address ON store.address_id = address.address_id
  JOIN city ON address.city_id = city.city_id
  JOIN customer ON staff.staff_id = customer.store_id
GROUP BY
  staff.staff_id
HAVING
  COUNT(customer.customer_id) > 300;

 /*Задание 2*/
SELECT COUNT(*) AS film_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);

/*Задание 3*/
SELECT
  MONTH(payment_date) AS month,
  SUM(amount) AS total_payment_amount,
  COUNT(rental_id) AS rental_count
FROM
  payment
GROUP BY
  MONTH(payment_date)
ORDER BY
  total_payment_amount DESC
LIMIT 1;

/*Задание 4*/
SELECT
  staff.staff_id,
  CONCAT(staff.first_name, ' ', staff.last_name) AS full_name,
  COUNT(*) AS sales_count,
  CASE WHEN COUNT(*) > 8000 THEN 'Да' ELSE 'Нет' END AS Премия
FROM
  payment
  JOIN staff ON payment.staff_id = staff.staff_id
GROUP BY
  staff.staff_id;

/*Задание 5*/
SELECT film.film_id, film.title
FROM film
LEFT JOIN rental ON film.film_id = rental.inventory_id
WHERE rental.inventory_id IS NULL;
```
