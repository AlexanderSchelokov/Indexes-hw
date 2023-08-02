Задание 1
---
Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

**Ответ**

![Снимок35](https://github.com/AlexanderSchelokov/Indexes-hw/assets/121572590/510fd94a-65be-41b0-94ff-481a1608e58e)

***

Задание 2
---
Выполните explain analyze следующего запроса:

```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```

перечислите узкие места;

оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

**Ответ**

К узким местам можо отнести не правильность присоединения таблиц.

Чтобы оптимизировать запрос, сделал следующее:

- Использовал JOIN вместо перечисления таблиц через запятую.
  
- Использовал оператор BETWEEN вместо функции DATE.


![Снимок36](https://github.com/AlexanderSchelokov/Indexes-hw/assets/121572590/e483c5c2-4188-4cae-a7eb-43ec89890ef2)

```
SELECT CONCAT(c.last_name, ' ', c.first_name), SUM(p.amount)

FROM payment p

JOIN rental r ON p.rental_id = r.rental_id

JOIN customer c ON r.customer_id = c.customer_id

JOIN inventory i ON r.inventory_id = i.inventory_id

JOIN film f ON i.film_id = f.film_id

WHERE p.payment_date BETWEEN '2005-07-30 00:00:00' AND '2005-07-30 23:59:59'

group by c.last_name, c.first_name, c.customer_id;
```


***

Дополнительные задания (со звёздочкой*)

Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.


Задание 3*
---

Самостоятельно изучите, какие типы индексов используются в PostgreSQL. Перечислите те индексы, которые используются в PostgreSQL, а в MySQL — нет.

Приведите ответ в свободной форме.

**Ответ**

Сводная таблица типов индексов

Bitmap index, Partial index, Function based index


***
