# Домашнее задание к занятию "`SQL. Часть 2`" - `Клименко Олег`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
select concat(s.first_name , ' ', s.last_name),  c2.city, COUNT(c.customer_id) 
from staff s
join store s2 on s2.store_id = s.store_id 
join customer c on c.store_id = s2.store_id
join address a on a.address_id = s2.address_id 
join city c2 on c2.city_id = a.city_id 
group by s.staff_id, c2.city_id 
having COUNT(c.customer_id) > 300;
```

![](https://cdn.discordapp.com/attachments/1258765873305616414/1263752133736665159/image.png?ex=669b6064&is=669a0ee4&hm=a164464a184ef43ebea097ce8a0169907630f5767e5e1ba1f81dfd10b2e6c5d7&)

---

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
select COUNT(f.title)
from film f  
where f.`length`  >
  (select AVG(`length`) 
  from film);   
```

![](https://cdn.discordapp.com/attachments/1258765873305616414/1263752578685210624/image.png?ex=669b60ce&is=669a0f4e&hm=a5d30a83ea8b43c017af4146e9494a6dfb5ddf3733ddc7b5c3a8598c215e972e&)

---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```
select t1.amount_of_payments, t1.month_of_payments
from (
  select SUM(p.amount) amount_of_payments, DATE_FORMAT(p.payment_date, '%M %Y') month_of_payments 
  from sakila.payment p 
  group by DATE_FORMAT(p.payment_date, '%M %Y')) t1
order by t1.amount_of_payments desc  
limit 1;
```

![](https://cdn.discordapp.com/attachments/1258765873305616414/1263752988724301885/image.png?ex=669b612f&is=669a0faf&hm=b1f5444c2ae096857332a0fc3ab43f86b04b2ab4106c7495e28194c7262ae0dd&)

---

### Задание 4

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```
select t1.cp count_of_payments, t1.staff_id staff_id,
  case 
  	when t1.cp > 8000 then 'Да'
  	else 'Нет'
  end as bonus
from (
  select COUNT(payment_id) cp, staff_id  
  from sakila.payment p 
  group by staff_id ) t1;
```

![](https://cdn.discordapp.com/attachments/1258765873305616414/1263753597129326645/image.png?ex=669b61c0&is=669a1040&hm=b93c6ab6b04fa471d4ef813acf4b54748c60f23202bb5aed0c82514ad61918dc&)

---

### Задание 5

Найдите фильмы, которые ни разу не брали в аренду.

```
select f.title  from sakila.rental r
right join sakila.inventory i on i.inventory_id = r.inventory_id  
right join sakila.film f  on f.film_id = i.film_id 
where  r.rental_id is null;
```

![](https://cdn.discordapp.com/attachments/1258765873305616414/1263753951782637568/image.png?ex=669b6215&is=669a1095&hm=0c5e79f343b0b071a5a0b4079ff3cdbb2c6f6ec6154964de97f02bf9e920adb1&)

---
---
























