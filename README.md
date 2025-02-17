# Домашнее задание к занятию "SQL. Часть 1" - `Александра Бужор`

---

### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.

### Решение:

```sql
SELECT DISTINCT district
FROM address
WHERE district LIKE 'K%a' AND district NOT LIKE '% %';
```
```
district |
---------+
Kanagawa |
Kalmykia |
Kaduna   |
Karnataka|
Kütahya  |
Kerala   |
Kitaa    |
```

---

### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года включительно и стоимость которых превышает 10.00.

### Решение:
```sql
SELECT *
FROM payment
WHERE payment_date BETWEEN '2005-06-15' AND '2005-06-18 23:59:59'
AND amount > 10.00;
```
```
payment_id|customer_id|staff_id|rental_id|amount|payment_date       |last_update        |
----------+-----------+--------+---------+------+-------------------+-------------------+
       908|         33|       1|     1301| 10.99|2005-06-15 09:46:33|2006-02-15 22:12:36|
      7017|        260|       1|     2091| 10.99|2005-06-17 18:09:04|2006-02-15 22:14:58|
      8272|        305|       1|     2166| 11.99|2005-06-17 23:51:21|2006-02-15 22:15:47|
     12888|        477|       1|     2306| 10.99|2005-06-18 08:33:23|2006-02-15 22:19:46|
     13892|        516|       1|     1718| 10.99|2005-06-16 14:52:02|2006-02-15 22:20:47|
     14620|        544|       2|     1434| 10.99|2005-06-15 18:30:46|2006-02-15 22:21:35|
     15313|        572|       2|     1889| 10.99|2005-06-17 04:05:12|2006-02-15 22:22:22|
```
---

### Задание 3

Получите последние пять аренд фильмов.

### Решение:

```sql
SELECT *
FROM rental
ORDER BY rental_date DESC
LIMIT 5;
```
```
rental_id|rental_date        |inventory_id|customer_id|return_date|staff_id|last_update        |
---------+-------------------+------------+-----------+-----------+--------+-------------------+
    11739|2006-02-14 15:16:03|        4568|        373|           |       2|2006-02-15 21:30:53|
    14616|2006-02-14 15:16:03|        4537|        532|           |       1|2006-02-15 21:30:53|
    11676|2006-02-14 15:16:03|        4496|        216|           |       2|2006-02-15 21:30:53|
    15966|2006-02-14 15:16:03|        4472|        374|           |       1|2006-02-15 21:30:53|
    13486|2006-02-14 15:16:03|        4460|        274|           |       1|2006-02-15 21:30:53|
```

---

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie.

Сформируйте вывод в результат таким образом:

все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
замените буквы 'll' в именах на 'pp'.

### Решение:

```sql
SELECT 
    REPLACE(LOWER(first_name), 'll', 'pp') AS first_name_modified,
    LOWER(last_name) AS last_name_modified
FROM customer
WHERE 
    (first_name = 'Kelly' OR first_name = 'Willie')
    AND active = 1;
```
```
first_name_modified|last_name_modified|
-------------------+------------------+
keppy              |torres            |
wippie             |howell            |
wippie             |markham           |
keppy              |knott             |
```

---

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.

### Решение:

```sql
SELECT 
    SUBSTRING_INDEX(email, '@', 1) AS email_username,
    SUBSTRING_INDEX(email, '@', -1) AS email_domain
FROM customer;
```
```
email_username      |email_domain      |
--------------------+------------------+
MARY.SMITH          |sakilacustomer.org|
PATRICIA.JOHNSON    |sakilacustomer.org|
LINDA.WILLIAMS      |sakilacustomer.org|
BARBARA.JONES       |sakilacustomer.org|
ELIZABETH.BROWN     |sakilacustomer.org|
JENNIFER.DAVIS      |sakilacustomer.org|
MARIA.MILLER        |sakilacustomer.org|
SUSAN.WILSON        |sakilacustomer.org|
MARGARET.MOORE      |sakilacustomer.org|
DOROTHY.TAYLOR      |sakilacustomer.org|
LISA.ANDERSON       |sakilacustomer.org|
NANCY.THOMAS        |sakilacustomer.org|
KAREN.JACKSON       |sakilacustomer.org|
BETTY.WHITE         |sakilacustomer.org|
HELEN.HARRIS        |sakilacustomer.org|
SANDRA.MARTIN       |sakilacustomer.org|
...
```

---

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.

### Решение:

```sql
SELECT 
    CONCAT(UPPER(SUBSTRING(SUBSTRING_INDEX(email, '@', 1), 1, 1)), LOWER(SUBSTRING(SUBSTRING_INDEX(email, '@', 1), 2))) AS email_username,
    CONCAT(UPPER(SUBSTRING(SUBSTRING_INDEX(email, '@', -1), 1, 1)), LOWER(SUBSTRING(SUBSTRING_INDEX(email, '@', -1), 2))) AS email_domain
FROM customer;
```
```
email_username    |email_domain      |
------------------+------------------+
Mary.smith        |Sakilacustomer.org|
Patricia.johnson  |Sakilacustomer.org|
Linda.williams    |Sakilacustomer.org|
Barbara.jones     |Sakilacustomer.org|
Elizabeth.brown   |Sakilacustomer.org|
Jennifer.davis    |Sakilacustomer.org|
Maria.miller      |Sakilacustomer.org|
Susan.wilson      |Sakilacustomer.org|
Margaret.moore    |Sakilacustomer.org|
Dorothy.taylor    |Sakilacustomer.org|
Lisa.anderson     |Sakilacustomer.org|
Nancy.thomas      |Sakilacustomer.org|
Karen.jackson     |Sakilacustomer.org|
Betty.white       |Sakilacustomer.org|
Helen.harris      |Sakilacustomer.org|
Sandra.martin     |Sakilacustomer.org|
...
```
