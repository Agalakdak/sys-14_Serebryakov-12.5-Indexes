# sys-12_Serebryakov-12.5-Indexes - Серебряков Руслан



### Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

```sql
SELECT (SUM(INDEX_LENGTH) * 100 ) / (SUM(DATA_LENGTH)) AS 'Отношение общего размера всех
индексов к общему
размеру всех таблиц
учебной БД (sakila)',
(select (SUM(INDEX_LENGTH) * 100 ) / (SUM(DATA_LENGTH))
FROM TABLES) as 'Отношение общего размера всех
индексов к общему
размеру всех таблиц'
FROM TABLES
WHERE TABLE_SCHEMA = 'sakila'
```

### Задание 2

Выполните explain analyze следующего запроса:
```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места;

Не совсем понятно зачем так много сравнений в WHERE, 

- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

```sql
select 
concat(c.last_name, ' ', c.first_name) as "Full Name", 
sum(p.amount) as "Total Amount"
from payment p 
inner join rental r on p.payment_date = r.rental_date
inner join customer c on r.customer_id = c.customer_id 
where date(p.payment_date) = '2005-07-30'
group by c.customer_id
```


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*

Самостоятельно изучите, какие типы индексов используются в PostgreSQL. Перечислите те индексы, которые используются в PostgreSQL, а в MySQL — нет.

*Приведите ответ в свободной форме.*

