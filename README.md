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
