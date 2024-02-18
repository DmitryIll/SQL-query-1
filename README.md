# Домашнее задание к занятию «SQL. Часть 1»

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.

```
-- Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
SELECT distinct district
FROM sakila.address
where district  like "K%a" 
and district not like "% %";
```


### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.

```
-- Получите из таблицы платежей за прокат фильмов информацию по платежам, 
-- которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно**
-- и стоимость которых превышает 10.00.

SELECT payment_id, customer_id, staff_id, rental_id, amount, payment_date, last_update 
FROM sakila.payment
where payment_date BETWEEN "2005-06-15" and "2005-06-19"
and amount > 10;
```

### Задание 3

Получите последние пять аренд фильмов.

```
-- Получите последние пять аренд фильмов.

SELECT rental_id, rental_date, inventory_id, customer_id, return_date, staff_id, last_update FROM sakila.rental
order by rental_date desc limit 5;
```

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.

```
SELECT customer_id, store_id, replace(LOWER(first_name),"ll","pp"), lower(last_name), email, address_id, active, create_date, last_update FROM sakila.customer
where first_name in ("Kelly", "Willie");
```


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.

```
SELECT customer_id, store_id, first_name, last_name,
-- email, 
left(email, POSITION("@" in email)-1), 
right(email, LENGTH(email)-POSITION("@" in email)),
address_id, active, create_date, last_update FROM sakila.customer;
```

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.

```
SELECT customer_id, store_id, first_name, last_name,
-- email, 
CONCAT(
  UPPER(
    LEFT(
     left(email, POSITION("@" in email)-1)
    ,1)
  ),
  LOWER(
    right(
      left(email, POSITION("@" in email)-1)
    ,
    LENGTH(left(email, POSITION("@" in email)-1))-1)
  )
), 
CONCAT(
  UPPER(
    LEFT( 
       right(email, LENGTH(email)-POSITION("@" in email))
    ,1)
  ),
  LOWER(
    right(
       right(email, LENGTH(email)-POSITION("@" in email))
    ,
      LENGTH(
        right(email, LENGTH(email)-POSITION("@" in email))
      )-1
    )  
  ) 
),
address_id, active, create_date, last_update FROM sakila.customer;
```