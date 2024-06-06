# Решения заданий из тренажера [sql academy](https://sql-academy.org/ru/trainer)

1. [Вывести имена всех людей, которые есть в базе данных авиакомпаний](https://sql-academy.org/ru/trainer/tasks/1)
```sql
SELECT name
FROM passenger;
```
2. [Вывести названия всеx авиакомпаний](https://sql-academy.org/ru/trainer/tasks/2)
```sql
SELECT name
FROM company;
```
3. [Вывести все рейсы, совершенные из Москвы](https://sql-academy.org/ru/trainer/tasks/3)
```sql
SELECT *
FROM trip
WHERE town_from = 'Moscow';
```
4. [Вывести имена людей, которые заканчиваются на "man"](https://sql-academy.org/ru/trainer/tasks/4)
```sql
SELECT name
FROM passenger
WHERE name LIKE '%man';
```
5. [Вывести количество рейсов, совершенных на TU-134](https://sql-academy.org/ru/trainer/tasks/5)
```sql
SELECT COUNT(*) AS count
FROM trip
WHERE plane = 'TU-134';
```
6. [Какие компании совершали перелеты на Boeing](https://sql-academy.org/ru/trainer/tasks/6)
```sql
SELECT DISTINCT name
FROM company
	JOIN trip ON company.id = trip.company
WHERE plane = 'Boeing';
```
7. [Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)](https://sql-academy.org/ru/trainer/tasks/7)
```sql
SELECT DISTINCT  plane FROM trip
WHERE town_to = 'Moscow';
```
8. [В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?](https://sql-academy.org/ru/trainer/tasks/8)
```sql
SELECT town_to, 
   TIMEDIFF(time_in, time_out) AS flight_time 
   FROM trip
WHERE town_from = 'Paris';
```
9. [Какие компании организуют перелеты из Владивостока (Vladivostok)?](https://sql-academy.org/ru/trainer/tasks/9)
```sql
SELECT name FROM company
    JOIN trip ON company.id = trip.company
WHERE trip.town_from = 'Vladivostok';
```
10. [Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.](https://sql-academy.org/ru/trainer/tasks/10)
```sql
SELECT * FROM trip
WHERE time_out BETWEEN '1900-01-01T10:00:00.000Z' AND '1900-01-01T14:00:00.000Z';
```
11. [Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.](https://sql-academy.org/ru/trainer/tasks/11)
```sql
SELECT name 
FROM passenger
WHERE LENGTH(name) = (
    SELECT MAX(LENGTH(name)) 
    FROM passenger
);
```
12. [Вывести id и количество пассажиров для всех прошедших полётов](https://sql-academy.org/ru/trainer/tasks/12)
```sql
SELECT trip,
       COUNT(*) AS count
FROM passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
GROUP BY trip;
```
13. [Вывести имена людей, у которых есть полный тёзка среди пассажиров](https://sql-academy.org/ru/trainer/tasks/13)
```sql
SELECT name
FROM passenger
GROUP BY name
HAVING COUNT(name) > 1;
```
14. [В какие города летал Bruce Willis](https://sql-academy.org/ru/trainer/tasks/14)
```sql
SELECT town_to
FROM passenger ps
     JOIN Pass_in_trip pt ON ps.id = pt.passenger
     JOIN trip tr ON tr.id = pt.trip
WHERE name = 'Bruce Willis';
```
15. [Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London)](https://sql-academy.org/ru/trainer/tasks/15)
```sql
SELECT time_in FROM trip
    JOIN Pass_in_trip AS pt ON trip.id = pt.trip
    JOIN passenger AS p ON p.id = pt.passenger
WHERE name = 'Steve Martin' AND town_to = 'London';
```
16. [Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.](https://sql-academy.org/ru/trainer/tasks/16)
```sql
SELECT name,
       COUNT(name) AS count
FROM passenger ps
     JOIN Pass_in_trip pt ON ps.id = pt.passenger
     JOIN trip tr ON pt.trip = tr.id
GROUP BY name
ORDER BY count DESC,
         name ASC;
```
17. [Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.](https://sql-academy.org/ru/trainer/tasks/17)
```sql
SELECT member_name,
       status,
       SUM(unit_price * amount) AS costs;
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
WHERE YEAR(DATE) = 2005
GROUP BY member_name,
         status;
```
18. [Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.](https://sql-academy.org/ru/trainer/tasks/18)
```sql
SELECT member_name 
FROM FamilyMembers
WHERE birthday = (SELECT MIN(birthday) 
                    FROM FamilyMembers);
```
19. [Определить, кто из членов семьи покупал картошку (potato)](https://sql-academy.org/ru/trainer/tasks/18)
```sql
SELECT DISTINCT status 
FROM FamilyMembers as fm
JOIN Payments AS p
	ON fm.member_id = p.family_member
JOIN Goods AS g
	ON p.good = g.good_id
WHERE g.good_name = 'potato';
```
20. [Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму](https://sql-academy.org/ru/trainer/tasks/20)
```sql
SELECT fm.status, 
        fm.member_name, 
        SUM(p.amount*p.unit_price) AS costs
FROM FamilyMembers AS fm
JOIN Payments AS p 
    ON p.family_member = fm.member_id
JOIN Goods as g 
    ON p.good = g.good_id
JOIN GoodTypes AS gt 
    ON gt.good_type_id = g.type
WHERE good_type_name = 'entertainment'
GROUP BY fm.status, fm.member_name;
```
21. [Определить товары, которые покупали более 1 раза](https://sql-academy.org/ru/trainer/tasks/21)
```sql
SELECT g.good_name 
FROM Goods AS g
JOIN Payments AS p
    ON p.good = g.good_id
GROUP BY g.good_name
HAVING COUNT(p.good) > 1;
```
22. [Найти имена всех матерей (mother)](https://sql-academy.org/ru/trainer/tasks/22)
```sql
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother';
```
23. [Найдите самый дорогой деликатес (delicacies) и выведите его цену](https://sql-academy.org/ru/trainer/tasks/23)
```sql
SELECT g.good_name, p.unit_price
FROM Goods AS g
JOIN Payments AS p
    ON g.good_id = p.good
JOIN GoodTypes AS gt
    ON g.type = gt.good_type_id
WHERE p.unit_price = (
    SELECT MAX(p2.unit_price)
    FROM Payments AS p2
    JOIN Goods AS g2 ON g2.good_id = p2.good
    JOIN GoodTypes AS gt2 ON g2.type = gt2.good_type_id
    WHERE gt2.good_type_name = 'delicacies'
);


OR


SELECT good_name,
       unit_price
FROM Goods gs
         JOIN GoodTypes gt ON gs.type = gt.good_type_id
         JOIN Payments ps ON gs.good_id = ps.good
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC
LIMIT 1;
```
24. [Определить кто и сколько потратил в июне 2005](https://sql-academy.org/ru/trainer/tasks/24)
```sql
SELECT member_name, SUM(amount*unit_price) AS costs
FROM FamilyMembers AS fm
JOIN Payments AS p 
    ON fm.member_id = p.family_member
WHERE MONTH(date) = 06 AND YEAR(date) = 2005
GROUP BY member_name;
```
25. [Определить, какие товары не покупались в 2005 году](https://sql-academy.org/ru/trainer/tasks/25)
```sql
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
    SELECT good
    FROM Payments
    WHERE YEAR(date) = 2005
);
```
26. [Определить группы товаров, которые не приобретались в 2005 году](https://sql-academy.org/ru/trainer/tasks/26)
```sql
SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN (
    SELECT type
    FROM Goods gs
             JOIN Payments ps ON gs.good_id = ps.good
    WHERE YEAR(date) = 2005
    GROUP BY good_id
);
```
27. [Узнать, сколько потрачено на каждую из групп товаров в 2005 году. Вывести название группы и сумму](https://sql-academy.org/ru/trainer/tasks/27)
```sql
SELECT good_type_name,
       SUM(amount * unit_price) AS costs
FROM GoodTypes gt
         JOIN Goods gs ON gt.good_type_id = gs.type
         JOIN Payments ps ON gs.good_id = ps.good
WHERE YEAR(date) = 2005
GROUP BY good_type_name;
```
28. [Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?](https://sql-academy.org/ru/trainer/tasks/28)
```sql
SELECT COUNT(*) AS COUNT
FROM Trip
WHERE town_from = 'Rostov'
  AND town_to = 'Moscow';
```
29. [Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134](https://sql-academy.org/ru/trainer/tasks/29)
```sql
SELECT DISTINCT name
FROM Passenger ps
         JOIN Pass_in_trip pt ON ps.id = pt.passenger
         JOIN Trip tr ON pt.trip = tr.id
WHERE plane = 'TU-134'
  AND town_to = 'Moscow';
```
30. [Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.](https://sql-academy.org/ru/trainer/tasks/30)
```sql
SELECT trip,
       COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```
31. [Вывести всех членов семьи с фамилией Quincey.](https://sql-academy.org/ru/trainer/tasks/31)
```sql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '% Quincey';
```
32. [Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.](https://sql-academy.org/ru/trainer/tasks/32)
```sql
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, CURRENT_DATE))) AS age
FROM FamilyMembers;
```
33. [Найдите среднюю стоимость икры. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar).](https://sql-academy.org/ru/trainer/tasks/33)
```sql
SELECT AVG(unit_price) AS cost
FROM Payments ps
         JOIN Goods gs ON ps.good = gs.good_id
WHERE good_name = 'red caviar'
   OR good_name = 'black caviar';
```
34. [Сколько всего 10-ых классов](https://sql-academy.org/ru/trainer/tasks/34)
```sql
SELECT COUNT(name) AS count
FROM Class
WHERE name LIKE '10 %';
```
