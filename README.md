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
35. [Сколько различных кабинетов школы использовались 2.09.2019 в образовательных целях ?](https://sql-academy.org/ru/trainer/tasks/35)
```sql
SELECT COUNT(classroom) AS count
FROM Schedule
WHERE date = '2019-09-02';
```
36. [Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?](https://sql-academy.org/ru/trainer/tasks/36)
```sql
SELECT *
FROM Student
WHERE address LIKE 'ul. Pushkina%';
```
37. [Сколько лет самому молодому обучающемуся ?](https://sql-academy.org/ru/trainer/tasks/37)
```sql
SELECT TIMESTAMPDIFF(YEAR, birthday, CURRENT_TIMESTAMP) AS year
FROM Student
ORDER BY year ASC
LIMIT 1;
```
38. [Сколько Анн (Anna) учится в школе ?](https://sql-academy.org/ru/trainer/tasks/38)
```sql
SELECT COUNT(*) AS count
FROM Student
WHERE first_name = 'Anna';
```
39. [Сколько обучающихся в 10 B классе ?](https://sql-academy.org/ru/trainer/tasks/39)
```sql
SELECT COUNT(*) AS count
FROM Student_in_class sc
         JOIN Class cl ON sc.class = cl.id
WHERE name = '10 B';
```
40. [Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.) ?](https://sql-academy.org/ru/trainer/tasks/40)
```sql
SELECT name AS subjects
FROM Subject 
WHERE id IN (
    SELECT subject 
    FROM Schedule AS s
    JOIN Teacher AS t 
        ON t.id = s.teacher
    WHERE t.first_name LIKE 'P%' 
        AND t.middle_name LIKE 'P%'
        AND t.last_name = 'Romashkin'
);
```
41. [Во сколько начинается 4-ый учебный предмет по расписанию ?](https://sql-academy.org/ru/trainer/tasks/41)
```sql
SELECT start_pair
FROM Timepair
WHERE id = 4;
```
42. [Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет](https://sql-academy.org/ru/trainer/tasks/42)
```sql
SELECT DISTINCT TIMEDIFF(
                (SELECT end_pair 
                    FROM Timepair 
                    WHERE id = 4),
                (SELECT start_pair 
                    FROM Timepair 
                    WHERE id = 2)) AS time
FROM Timepair;
```
43. [Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке](https://sql-academy.org/ru/trainer/tasks/43)
```sql
SELECT last_name 
FROM Teacher AS t
JOIN Schedule AS s 
    ON s.teacher = t.id
JOIN Subject AS st 
    ON st.id = s.subject
WHERE st.name = 'Physical Culture'
ORDER BY last_name;
```
44. [Найдите максимальный возраст (колич. лет) среди обучающихся 10 классов ?](https://sql-academy.org/ru/trainer/tasks/44)
```sql
SELECT TIMESTAMPDIFF(YEAR, birthday, CURRENT_TIMESTAMP) AS max_year
FROM Student st
         JOIN Student_in_class sc ON sc.student = st.id
         JOIN Class cl ON cl.id = sc.class
WHERE name LIKE '10 %'
ORDER BY max_year DESC
LIMIT 1;
```
45. [Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз.](https://sql-academy.org/ru/trainer/tasks/45)
```sql
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING count(classroom) = (
    SELECT COUNT(*) AS count
    FROM Schedule
    GROUP BY classroom
    ORDER BY count DESC
    LIMIT 1
);
```
46. [В каких классах введет занятия преподаватель "Krauze" ?](https://sql-academy.org/ru/trainer/tasks/46)
```sql
SELECT name
FROM Schedule sc
         JOIN Teacher tc ON tc.id = sc.teacher
         JOIN Class cl ON cl.id = sc.class
WHERE last_name = 'Krauze'
GROUP BY name;
```
47. [Сколько занятий провел Krauze 30 августа 2019 г.?](https://sql-academy.org/ru/trainer/tasks/47)
```sql
SELECT COUNT(*) AS count
FROM Schedule sc
         JOIN Teacher tc ON tc.id = sc.teacher
WHERE DATE_FORMAT(date, '%e %M %Y') = '30 August 2019'
  AND last_name = 'Krauze';
```
48. [Выведите заполненность классов в порядке убывания](https://sql-academy.org/ru/trainer/tasks/48)
```sql
SELECT name,
       COUNT(student) AS count
FROM Class cl
         JOIN Student_in_class sc ON sc.class = cl.id
GROUP BY name
ORDER BY count DESC;
```
49. [Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 без округления, например, 96.0201.](https://sql-academy.org/ru/trainer/tasks/49)
```sql
SELECT COUNT(*) * 100 / (
    SELECT COUNT(*)
    FROM Student_in_class
) AS percent
FROM Student_in_class sc
         JOIN Class cs ON cs.id = sc.class
WHERE name = '10 A';
```
50. [Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.](https://sql-academy.org/ru/trainer/tasks/50)
```sql
SELECT FLOOR(
                       COUNT(*) * 100 / (
                   SELECT COUNT(*)
                   FROM Student_in_class
               )
           ) AS percent
FROM Student_in_class sc
         JOIN Student st ON st.id = sc.student
WHERE YEAR(birthday) = 2000;
```
51. [Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).](https://sql-academy.org/ru/trainer/tasks/51)
```sql
INSERT INTO Goods
SET good_id   = (
    SELECT COUNT(*) + 1
    FROM Goods AS gs
),
    good_name = 'Cheese',
    type      = (
        SELECT good_type_id
        FROM GoodTypes
        WHERE good_type_name = 'food'
    );
```
52. [Добавьте в список типов товаров (GoodTypes) новый тип "auto".](https://sql-academy.org/ru/trainer/tasks/52)
```sql
INSERT INTO GoodTypes
SET good_type_id   = (
    SELECT COUNT(*) + 1
    FROM GoodTypes AS gt
),
    good_type_name = 'auto';
```
53. [Измените имя "Andie Quincey" на новое "Andie Anthony".](https://sql-academy.org/ru/trainer/tasks/53)
```sql
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```
54. [Удалить всех членов семьи с фамилией "Quincey".](https://sql-academy.org/ru/trainer/tasks/54)
```sql
DELETE
FROM FamilyMembers
WHERE member_name LIKE '% Quincey';
```
55. [Удалить компании, совершившие наименьшее количество рейсов.](https://sql-academy.org/ru/trainer/tasks/55)
```sql
DELETE
FROM company
WHERE id IN (
    SELECT company
    FROM trip
    GROUP BY company
    HAVING COUNT(*) = (
        SELECT COUNT(*) AS count
        FROM trip
        GROUP BY company
        ORDER BY count
        LIMIT 1
    )
);
```
56. [Удалить все перелеты, совершенные из Москвы (Moscow).](https://sql-academy.org/ru/trainer/tasks/56)
```sql
DELETE
FROM trip
WHERE town_from = 'Moscow';
```
57. [Перенести расписание всех занятий на 30 мин. вперед.](https://sql-academy.org/ru/trainer/tasks/57)
```sql
UPDATE Timepair
SET start_pair = ADDTIME(start_pair, '00:30:00'),
    end_pair   = ADDTIME(end_pair, '00:30:00');
```
58. [Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George Clooney"](https://sql-academy.org/ru/trainer/tasks/58)
```sql
INSERT INTO Reviews
SET id             = (
    SELECT COUNT(*) + 1
    FROM Reviews rw
),
    reservation_id = (
        SELECT rs.id
        FROM Reservations rs
                 JOIN Rooms rm ON rm.id = rs.room_id
                 JOIN Users us ON rs.user_id = us.id
        WHERE address = '11218, Friel Place, New York'
          AND name = 'George Clooney'
    ),
    rating = 5;
```
59. [Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375.](https://sql-academy.org/ru/trainer/tasks/59)
```sql
SELECT *
FROM Users
WHERE phone_number LIKE '+375%';
```
60. [Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых классов.](https://sql-academy.org/ru/trainer/tasks/60)
```sql
SELECT teacher
FROM Schedule sc
         JOIN Class cl ON sc.class = cl.id
WHERE name LIKE '11 %'
GROUP BY teacher
HAVING COUNT(DISTINCT name) = 2;
```
61. [Выведите список комнат, которые были зарезервированы хотя бы на одни сутки в 12-ую неделю 2020 года. В данной задаче в качестве одной недели примите период из семи дней, первый из которых начинается 1 января 2020 года. Например, первая неделя года — 1–7 января, а третья — 15–21 января.](https://sql-academy.org/ru/trainer/tasks/61)
```sql
SELECT Rooms.*
FROM Reservations
         JOIN Rooms ON Rooms.id = Reservations.room_id
WHERE WEEK(start_date, 1) = 12
  AND YEAR(start_date) = 2020;
```
62. [Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён.](https://sql-academy.org/ru/trainer/tasks/62)
```sql
SELECT SUBSTRING_INDEX(email, '@', -1)        AS domain,
       COUNT(substring_index(email, '@', -1)) AS count
FROM Users
GROUP BY domain
ORDER BY count DESC,
         domain;
```
63. [Выведите отсортированный список (по возрастанию) фамилий и имен студентов в виде Фамилия.И.](https://sql-academy.org/ru/trainer/tasks/63)
```sql
SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.') AS name
FROM Student
ORDER BY name;
```
64. [Вывести количество бронирований по каждому месяцу каждого года, в которых было хотя бы 1 бронирование. Результат отсортируйте в порядке возрастания даты бронирования.](https://sql-academy.org/ru/trainer/tasks/64)
```sql
SELECT YEAR(start_date)  AS year,
       MONTH(start_date) AS month,
       COUNT(*)          AS amount
FROM Reservations
GROUP BY YEAR(start_date),
         MONTH(start_date)
ORDER BY year,
         month;
```
65. [Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов округленное до целого вниз.](https://sql-academy.org/ru/trainer/tasks/65)
```sql
SELECT Reservations.room_id, FLOOR(AVG(Reviews.rating)) AS rating
FROM Reviews 
JOIN Reservations
    ON Reservations.id = Reviews.reservation_id
GROUP BY Reservations.room_id;
```
66. [Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество дней и сумму за все дни аренды каждой из таких комнат.](https://sql-academy.org/ru/trainer/tasks/66)
```sql
SELECT home_type,
       address,
       IFNULL(SUM(total / rs.price), 0) AS days,
       IFNULL(SUM(total), 0)            AS total_fee
FROM Rooms AS rm
LEFT JOIN Reservations AS rs 
    ON rs.room_id = rm.id
WHERE has_tv = 1
  AND has_internet = 1
  AND has_kitchen = 1
  AND has_air_con = 1
GROUP BY home_type,
         address;
```
67. [Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты с ведущим нулем, а день и месяц без.](https://sql-academy.org/ru/trainer/tasks/67)
```sql
SELECT CONCAT(
       DATE_FORMAT(time_out, '%H:%i, %e.%c'),
       ' - ',
       DATE_FORMAT(time_in, '%H:%i, %e.%c')
   ) AS flight_time
FROM Trip;
```
68. [Для каждой комнаты, которую снимали как минимум 1 раз, найдите имя человека, снимавшего ее последний раз, и дату, когда он выехал](https://sql-academy.org/ru/trainer/tasks/68)
```sql
SELECT rs.room_id,
       name,
       date AS end_date
FROM (
         SELECT room_id,
                MAX(end_date) AS date
         FROM Reservations
         GROUP BY room_id
     ) rs
         JOIN Reservations rsv ON rs.room_id = rsv.room_id
    AND rs.date = rsv.end_date
         JOIN Users us ON rsv.user_id = us.id;
```
69. [Вывести идентификаторы всех владельцев комнат, что размещены на сервисе бронирования жилья и сумму, которую они заработали](https://sql-academy.org/ru/trainer/tasks/69)
```sql
SELECT owner_id,
       IFNULL(SUM(total), 0) AS total_earn
FROM Rooms rm
LEFT JOIN Reservations rs 
    ON rm.id = rs.room_id
GROUP BY owner_id;
```
70. [Необходимо категоризовать жилье на economy, comfort, premium по цене соответственно <= 100, 100 < цена < 200, >= 200. В качестве результата вывести таблицу с названием категории и количеством жилья, попадающего в данную категорию](https://sql-academy.org/ru/trainer/tasks/70)
```sql
SELECT CASE
       WHEN price <= 100 THEN 'economy'
       WHEN price > 100
           AND price < 200 THEN 'comfort'
       WHEN price >= 200 THEN 'premium'
       END      AS category,
   COUNT(price) AS count
FROM Rooms
GROUP BY category;
```
71. [Найдите какой процент пользователей, зарегистрированных на сервисе бронирования, хоть раз арендовали или сдавали в аренду жилье. Результат округлите до сотых.](https://sql-academy.org/ru/trainer/tasks/71)
```sql
SELECT ROUND(
       (
	   SELECT COUNT(*)
	   FROM (
		    SELECT DISTINCT owner_id
		    FROM Rooms rm
			     JOIN Reservations rs ON rm.id = rs.room_id
		    UNION
		    SELECT user_id
		    FROM Reservations
		) active_users
       ) * 100 / (
	   SELECT COUNT(*)
	   FROM Users
       ),
       2
) AS percent;
```
72. [Выведите среднюю стоимость бронирования для комнат, которых бронировали хотя бы один раз. Среднюю стоимость необходимо округлить до целого значения вверх.](https://sql-academy.org/ru/trainer/tasks/72)
```sql
SELECT room_id,
       CEILING(AVG(price)) AS avg_price
FROM Reservations
GROUP BY room_id;
```
73. [Выведите id тех комнат, которые арендовали нечетное количество раз](https://sql-academy.org/ru/trainer/tasks/73)
```sql
SELECT room_id,
       COUNT(*) AS count
FROM Reservations
GROUP BY room_id
HAVING count % 2 != 0;
```
74. [Выведите идентификатор и признак наличия интернета в помещении. Если интернет в сдаваемом жилье присутствует, то выведите «YES», иначе «NO».](https://sql-academy.org/ru/trainer/tasks/74)
```sql
SELECT id,
       IF(has_internet = 1, 'YES', 'NO') AS has_internet
FROM Rooms;
```
75. [Выведите фамилию, имя и дату рождения студентов, кто был рожден в мае.](https://sql-academy.org/ru/trainer/tasks/75)
```sql
SELECT last_name, first_name, birthday
FROM Student
WHERE birthday LIKE '%-05-%';
```
76. [Вывести имена всех пользователей сервиса бронирования жилья, а также два признака: является ли пользователь собственником какого-либо жилья (is_owner) и является ли пользователь арендатором (is_tenant). В случае наличия у пользователя признака необходимо вывести в соответствующее поле 1, иначе 0.](https://sql-academy.org/ru/trainer/tasks/76)
```sql
SELECT name,
       IF(
                   id IN (
                   SELECT owner_id
                   FROM Rooms
               ),
                   1,
                   0
           ) AS is_owner,
       IF(
                   id IN (
                   SELECT user_id
                   FROM Reservations
               ),
                   1,
                   0
           ) AS is_tenant
FROM Users;
```
77. [Создайте представление с именем "People", которое будет содержать список имен (first_name) и фамилий (last_name) всех студентов (Student) и преподавателей(Teacher)](https://sql-academy.org/ru/trainer/tasks/77)
```sql
CREATE VIEW People AS
SELECT first_name,
       last_name
FROM Student
UNION
SELECT first_name,
       last_name
FROM Teacher;
```
78. [Выведите всех пользователей с электронной почтой в «hotmail.com»](https://sql-academy.org/ru/trainer/tasks/78)
```sql
SELECT *
FROM Users
WHERE email LIKE '%@hotmail.com';
```
79. [Выведите поля id, home_type, price у всех комнат из таблицы Rooms. Если комната имеет телевизор и интернет одновременно, то в качестве цены в поле price выведите цену, применив скидку 10%](https://sql-academy.org/ru/trainer/tasks/79)
```sql
SELECT id,
       home_type,
       IF(has_tv AND has_internet, price * 0.9, price) AS price
FROM Rooms;
```
80. [Создайте представление «Verified_Users» с полями id, name и email, которое будет показывает только тех пользователей, у которых подтвержден адрес электронной почты.](https://sql-academy.org/ru/trainer/tasks/80)
```sql
CREATE VIEW Verified_Users AS
SELECT id, name, email
FROM Users
WHERE email_verified_at IS NOT NULL;
```
93. [Какой средний возраст клиентов, купивших Smartwatch (использовать наименование товара product.name) в 2024 году?](https://sql-academy.org/ru/trainer/tasks/93)
```sql
SELECT AVG(uniqueCustomers.age) AS average_age
FROM (
      SELECT DISTINCT Customer.customer_key, Customer.age
      FROM Purchase
	JOIN Customer ON Purchase.customer_key = Customer.customer_key
	JOIN Product ON Purchase.product_key = Product.product_key
	WHERE Product.name = 'Smartwatch'
		AND YEAR(Purchase.date) = 2024
     ) AS uniqueCustomers
```
94. [Вывести имена покупателей, каждый из которых приобрёл Laptop и Monitor (использовать наименование товара product.name) в марте 2024 года?](https://sql-academy.org/ru/trainer/tasks/94)
```sql
SELECT c.name
FROM Customer AS c
JOIN Purchase AS p
	ON c.customer_key = p.customer_key
JOIN Product AS pt
    ON p.product_key = pt.product_key
WHERE pt.name IN ('Laptop', 'Monitor')
	AND MONTH(p.date) = 3
	AND YEAR(p.date) = 2024
GROUP BY c.customer_key
HAVING COUNT(DISTINCT pt.name) = 2;
```
97. [Посчитать количество работающих складов на текущую дату по каждому городу. Вывести только те города, у которых количество складов более 80. Данные на выходе - город, количество складов.](https://sql-academy.org/ru/trainer/tasks/97)
```sql
SELECT city, COUNT(*) AS warehouse_count
FROM Warehouses
WHERE current_date BETWEEN date_open AND date_close
GROUP BY city
HAVING COUNT(*) > 80;
```
99. [Посчитай доход с женской аудитории (доход = сумма(price * items)). Обратите внимание, что в таблице женская аудитория имеет поле user_gender «female» или «f».](https://sql-academy.org/ru/trainer/tasks/99)
```sql
SELECT SUM(price*items) AS income_from_female
FROM Purchases
WHERE user_gender IN ('f', 'female');
```
101. [Выведи для каждого пользователя первое наименование, которое он заказал (первое по времени транзакции).](https://sql-academy.org/ru/trainer/tasks/101)
```sql
SELECT user_id, item
FROM (
    SELECT user_id, item,
           ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY transaction_ts) AS rn
    FROM Transactions
) t
WHERE t.rn = 1;
```
103. [Вывести список имён сотрудников, получающих большую заработную плату, чем у непосредственного руководителя.](https://sql-academy.org/ru/trainer/tasks/103)
```sql
SELECT e.NAME AS name
FROM EMPLOYEE e
JOIN EMPLOYEE AS c 
    ON e.CHIEF_ID = c.ID
WHERE e.SALARY > c.SALARY;
```
