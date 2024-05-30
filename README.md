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
       SUM(unit_price * amount) AS costs
FROM FamilyMembers fm
         JOIN Payments ps ON fm.member_id = ps.family_member
WHERE YEAR(DATE) = 2005
GROUP BY member_name,
         status;
```
