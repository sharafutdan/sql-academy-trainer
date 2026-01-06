# Решения заданий тренажёра от [sql-academy](https://sql-academy.org)

<details>
<summary>Задание 1 - Имена всех людей</summary>

**Описание задачи:**
Вывести имена всех людей, которые есть в базе данных авиакомпании.

**Решение:**

```sql
SELECT name FROM Passenger;
```

</details>

<details>
<summary>Задание 2 - Названия всех авиакомпаний</summary>

**Описание задачи:**
Вывести названия всеx авиакомпаний
Поля в результирующей таблице:
name

**Решение:**

```sql
SELECT c.name
FROM Company c
```

</details>

<details>
<summary>Задание 3 - Рейсы из Москвы</summary>

**Описание задачи:**
Вывести все рейсы, совершенные из Москвы
Поля в результирующей таблице:
все

**Решение:**

```sql
SELECT *
FROM Trip t
WHERE t.town_from = 'Moscow'
```

</details>

<details>
<summary>Задание 4 - Имена, заканчивающиеся на "man"</summary>

**Описание задачи:**
Вывести имена людей, которые заканчиваются на "man"

**Решение:**

```sql
SELECT *
FROM Trip t
WHERE t.town_from = 'Moscow'
```

</details>

<details>
<summary>Задание 5 - Количество рейсов на TU-134</summary>

**Описание задачи:**
Вывести количество рейсов, совершенных на TU-134
Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
count

**Решение:**

```sql
SELECT COUNT(t.id) as count
FROM Trip t
WHERE t.plane = 'TU-134'
```

</details>

<details>
<summary>Задание 6 - Компании, летавшие на Boeing</summary>

**Описание задачи:**
Какие компании совершали перелеты на Boeing

**Решение:**

```sql
SELECT DISTINCT
    c.name
FROM Company c
JOIN Trip t on t.company = c.id and t.plane LIKE '%Boeing%'
```

```sql
SELECT DISTINCT ON (c.id)
        c.ame
FROM Company c
JOIN Trip t on c.id = t.company
WHERE t.plane LIKE '%Boeing%'
```

</details>

<details>
<summary>Задание 7 - Самолеты, летящие в Москву</summary>

**Описание задачи:**
Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
Поля в результирующей таблице:
plane

**Решение:**

```sql
SELECT DISTINCT
    t.plane
FROM Trip t
WHERE t.town_to='Moscow'
```

</details>

<details>
<summary>Задание 8 - Полёты из Парижа</summary>

**Описание задачи:**
В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?(Поля в результирующей таблице:
town_to
flight_time)

**Решение:**

```sql
SELECT DISTINCT
    t.town_to,
	timediff(t.time_in, t.time_out) as flight_time
FROM Trip t
WHERE t.town_from = 'Paris'
```

</details>

<details>
<summary>Задание 9 - Компании с рейсами из Владивостока</summary>

**Описание задачи:**
Какие компании организуют перелеты из Владивостока (Vladivostok)?

**Решение:**

```sql
SELECT DISTINCT c.name
FROM Company c
	JOIN Trip t on t.company = c.id
	AND t.town_from = 'Vladivostok'
```

```sql
SELECT DISTINCT c.name
FROM Company c
WHERE c.id in (
		SELECT t.company
		FROM Trip t
		WHERE t.town_from = 'Vladivostok'
	)
```

</details>

<details>
<summary>Задание 10 - Вылеты в определенное время</summary>

**Описание задачи:**
Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.

**Решение:**

```sql
SELECT *
FROM Trip t
WHERE t.time_out BETWEEN '1900-01-01 10:00' AND '1900-01-01 14:00'
```

</details>

<details>
<summary>Задание 11 - Пассажиры с самым длинным ФИО</summary>

**Описание задачи:**
Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.

**Решение:**

```sql
SELECT
    p.name
FROM Passenger p
WHERE LENGTH(name) = (SELECT MAX(LENGTH(name)) FROM Passenger);
```

```sql
SELECT p.name
FROM Passenger p
	JOIN (
		SELECT MAX(LENGTH(p2.name)) name_max_len
		FROM Passenger p2
	) p2 ON LENGTH(p.name) = p2.name_max_len
```

</details>

<details>
<summary>Задание 12 - Количество пассажиров на рейсах</summary>

**Описание задачи:**
Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".

**Решение:**

```sql
SELECT t.id,
	COUNT(pt.passenger)
FROM Trip t
LEFT JOIN Pass_in_trip pt ON pt.trip = t.id
GROUP BY t.id
```

</details>

<details>
<summary>Задание 13 - Полные тёзки</summary>

**Описание задачи:**
Вывести имена людей, у которых есть полный тёзка среди пассажиров

**Решение:**

```sql
SELECT DISTINCT
    p.name
FROM Passenger p
JOIN (
    SELECT
        p2.id,
        p2.name
    FROM Passenger p2
) p2 ON p.id != p2.id and p.name = p2.name
```

```sql
SELECT DISTINCT
    p.name
FROM Passenger p
WHERE EXISTS(
    SELECT
        1
    FROM Passenger p2
    WHERE p.name = p2.name and p.id != p2.id
)
```

</details>

<details>
<summary>Задание 14 - Города, которые посетил Bruce Willis</summary>

**Описание задачи:**
В какие города летал Bruce Willis

**Решение:**

```sql
SELECT DISTINCT
    t.town_to
FROM Trip t
JOIN Pass_in_trip pt ON t.id = pt.trip
JOIN Passenger p on pt.passenger = p.id and p.name = 'Bruce Willis'
```

</details>

<details>
<summary>Задание 15 - Прибытие Steve Martin в Лондон</summary>

**Описание задачи:**
Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London)

**Решение:**

```sql
SELECT p.id,
	t.time_in
FROM Passenger p
	JOIN Pass_in_trip pt on pt.passenger = p.id
	JOIN Trip t on t.id = pt.trip
WHERE p.name = 'Steve Martin'
	and t.town_to = 'London'
```

</details>

<details>
<summary>Задание 16 - Сортировка пассажиров по количеству полетов</summary>

**Описание задачи:**
Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.

**Решение:**

```sql
SELECT p.name as name,
	COUNT(p.id) as count
FROM Passenger p
	JOIN Pass_in_trip pt on pt.passenger = p.id
GROUP BY p.id
HAVING COUNT(p.id) > 0
ORDER BY count DESC, name ASC
```

</details>

<details>
<summary>Задание 17 - Траты членов семьи в 2005 году</summary>

**Описание задачи:**
Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.

**Решение:**

```sql
SELECT fm.member_name,
	fm.status,
	SUM(p.unit_price * p.amount) as costs
FROM FamilyMembers fm
	JOIN Payments p ON fm.member_id = p.family_member
	and EXTRACT(
		year
		from p.date
	) = 2005
GROUP BY fm.member_id
```

</details>

<details>
<summary>Задание 18 - Самый старший человек</summary>

**Описание задачи:**
Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.

**Решение:**

```sql
SELECT member_name
FROM FamilyMembers
WHERE birthday = (SELECT MIN(birthday) FROM FamilyMembers);
```

```sql
SELECT fm.member_name
FROM FamilyMembers fm
JOIN (
    SELECT MIN(birthday) as min_birthday
    FROM FamilyMembers
) fm2 ON fm.birthday = fm2.min_birthday;
```

</details>

<details>
<summary>Задание 19 - Кто покупал картошку</summary>

**Описание задачи:**
Определить, кто из членов семьи покупал картошку (potato)

**Решение:**
```sql
SELECT DISTINCT
    fm.status
FROM FamilyMembers fm
JOIN Payments p ON p.family_member = fm.member_id
JOIN Goods g ON g.good_id = p.good and g.good_name = 'potato'
```

</details>

<details>
<summary>Задание 20 - Траты на развлечения</summary>

**Описание задачи:**
Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму

**Решение:**
```sql
SELECT
    fm.status,
    fm.member_name,
    SUM(p.unit_price * p.amount) costs
FROM FamilyMembers fm
JOIN Payments p ON p.family_member = fm.member_id
JOIN Goods g ON g.good_id = p.good
JOIN GoodTypes gt ON gt.good_type_id = g.type AND gt.good_type_name = 'entertainment'
GROUP BY fm.member_id
```

</details>

<details>
<summary>Задание 21 - Товары, купленные более одного раза</summary>

**Описание задачи:**
Определить товары, которые покупали более 1 раза

**Решение:**
```sql
SELECT g.good_name
FROM Goods g
         JOIN Payments p ON p.good = g.good_id
GROUP BY g.good_id
HAVING COUNT(p.payment_id) > 1
```
</details>

<details>
<summary>Задание 22 - Имена всех матерей</summary>

**Описание задачи:**
Найти имена всех матерей (mother

**Решение:**
```sql
SELECT fm.member_name
FROM FamilyMembers fm
WHERE fm.status = 'mother'
```
</details>

<details>
<summary>Задание 23 - Самый дорогой деликатес</summary>

**Описание задачи:**
Найдите самый дорогой деликатес (delicacies) и выведите его цену

**Решение:**
```sql
SELECT g.good_name,
       p.unit_price
FROM Goods g
         JOIN Payments p ON g.good_id = p.good
         JOIN GoodTypes gt ON gt.good_type_id = g.type
WHERE gt.good_type_name = 'delicacies'
ORDER BY p.unit_price DESC
LIMIT 1;
```
```sql
SELECT g.good_name,
	p.unit_price
FROM Goods g
	JOIN GoodTypes gt ON g.type = gt.good_type_id
	JOIN Payments p ON g.good_id = p.good
	JOIN (
		SELECT MAX(p2.unit_price) as max_price
		FROM Payments p2
			JOIN Goods g2 ON p2.good = g2.good_id
			JOIN GoodTypes gt2 ON g2.type = gt2.good_type_id
		WHERE gt2.good_type_name = 'delicacies'
	) mp ON p.unit_price = mp.max_price
```
</details>

<details>
<summary>Задание 24 - Кто и сколько потратил в июне 2005 года</summary>

**Описание задачи:**
Определить кто и сколько потратил в июне 2005

**Решение:**
```sql
SELECT fm.member_name,
       SUM(p.unit_price * p.amount) as costs
FROM FamilyMembers fm
         JOIN Payments p ON p.family_member = fm.member_id
WHERE YEAR(p.date) = 2005
  and MONTH(p.date) = 6
GROUP BY fm.member_name
```
</details>

<details>
<summary>Задание 25 - Товары, не купленные в 2005 году</summary>

**Описание задачи:**
Определить, какие товары не покупались в 2005 году

**Решение:**
```sql
SELECT
    g.good_name
FROM Goods g
WHERE NOT EXISTS(
    SELECT
        1
    FROM Payments p
    WHERE p.good = g.good_id
      AND EXTRACT(YEAR FROM p.date) = 2005
)
```
```sql
SELECT DISTINCT g.good_name
FROM Goods g
         LEFT JOIN Payments p ON g.good_id = p.good
    AND EXTRACT(YEAR FROM p.date) = 2005
WHERE p.payment_id IS NULL;
```
</details>

<details>
<summary>Задание 26 - Группы товаров, не купленные в 2005 году</summary>

**Описание задачи:**
Определить группы товаров, которые не приобретались в 2005 году

**Решение:**
```sql
SELECT gt.good_type_name
FROM GoodTypes gt
WHERE NOT EXISTS (
    SELECT 1
    FROM Goods g
             JOIN Payments p ON p.good = g.good_id
    WHERE g.type = gt.good_type_id
      AND p.date BETWEEN '2005-01-01' AND '2005-12-31'
)
```
```sql
SELECT gt.good_type_name
FROM GoodTypes gt
WHERE gt.good_type_id NOT IN (
		SELECT g.type
		FROM Goods g
			JOIN Payments p on p.good = g.good_id
			and p.date BETWEEN '2005-01-01' and '2005-12-31'
	)
```
</details>

<details>
<summary>Задание 27 - Траты по группам товаров в 2005 году</summary>

**Описание задачи:**
Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.

**Решение:**
```sql
SELECT gt.good_type_name,
       SUM(p.amount * p.unit_price) costs
FROM GoodTypes gt
JOIN Goods g ON g.type = gt.good_type_id
JOIN Payments p ON p.good = g.good_id AND EXTRACT(YEAR FROM p.date) = 2005
GROUP BY gt.good_type_id
```
</details>

<details>
<summary>Задание 28 - Рейсы из Ростова в Москву</summary>

**Описание задачи:**
Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?
Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
count

**Решение:**

```sql
SELECT COUNT(*) as count
FROM Trip t
WHERE t.town_from = 'Rostov'
	and t.town_to = 'Moscow'
```

</details>

<details>
<summary>Задание 29 - Имена пассажиров, летящих в Москву</summary>

**Описание задачи:**
Выведите имена пассажиров, улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов.
Поля в результирующей таблице:
name

**Решение:**
```sql
SELECT DISTINCT
    p.name
FROM Passenger p
JOIN Pass_in_trip pt ON pt.passenger = p.id
JOIN Trip t ON t.id = pt.trip AND t.plane = 'TU-134' and t.town_to='Moscow'
```
</details>

<details>
<summary>Задание 30 - Нагруженность рейсов</summary>

**Описание задачи:**
Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.

**Решение:**
```sql
SELECT pt.trip trip,
       COUNT(pt.passenger) count
FROM Pass_in_trip pt
GROUP BY pt.trip
ORDER BY COUNT(pt.passenger) DESC
```
</details>

<details>
<summary>Задание 31 - Члены семьи Quincey</summary>

**Описание задачи:**
Вывести всех членов семьи с фамилией Quincey.

**Решение:**
```sql
SELECT *
FROM FamilyMembers fm
WHERE fm.member_name LIKE '%Quincey%'
```
</details>

<details>
<summary>Задание 32 - Средний возраст людей</summary>

**Описание задачи:**
Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.

**Решение:**
```sql
SELECT FLOOR(AVG(EXTRACT(YEAR FROM AGE(CURRENT_DATE, birthday)))) AS age
FROM FamilyMembers;
```
</details>

<details>
<summary>Задание 33 - Средняя цена икры</summary>

**Описание задачи:**
Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments.
В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar).
В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры.

**Решение:**
```sql
SELECT
    AVG(p.unit_price) as cost
FROM Payments p
JOIN Goods g ON g.good_id = p.good AND g.good_name LIKE '%caviar%'
```
</details>

<details>
<summary>Задание 34 - Количество 10-х классов</summary>

**Описание задачи:**
Сколько всего 10-ых классов

**Решение:**
```sql
SELECT
    COUNT(c.id)
FROM Class c
WHERE c.name LIKE '10%'
```
</details>

<details>
<summary>Задание 35 - Кабинеты, использованные 2 сентября 2019</summary>

**Описание задачи:**
Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?

**Решение:**
```sql
SELECT
    COUNT(DISTINCT s.classroom)
FROM Schedule s
WHERE s.date = '2019-09-02'

```
</details>

<details>
<summary>Задание 36 - Обучающиеся, живущие на улице Пушкина</summary>

**Описание задачи:**
Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?

**Решение:**
```sql
SELECT
    *
FROM Student s
WHERE s.address like '%ul. Pushkina%'
```

</details>

<details>
<summary>Задание 37 - Возраст самого молодого обучающегося</summary>

**Описание задачи:**
Сколько лет самому молодому обучающемуся ?

**Решение:**
```sql
SELECT
    MIN(EXTRACT(YEAR FROM AGE(CURRENT_DATE, s.birthday))) AS year
FROM Student s;
```
</details>

<details>
<summary>Задание 38 - Количество учениц с именем Анна</summary>

**Описание задачи:**
Сколько учениц с именем Анна (Anna) учится в школе?

**Решение:**
```sql
SELECT
    COUNT(s.id)
FROM Student s
WHERE s.first_name LIKE '%Anna%'
```
</details>

<details>
<summary>Задание 39 - Количество обучающихся в 10 B классе</summary>

**Описание задачи:**
Сколько обучающихся в 10 B классе ?

**Решение:**
```sql
SELECT
    COUNT(s.id) count
FROM
    Student s
    JOIN Student_in_class sic on sic.student = s.id
    JOIN Class c on c.id = sic.class
    and c.name LIKE '%10 B%'
```
</details>

<details>
<summary>Задание 40 - Предметы Ромашкина П.П.</summary>

**Описание задачи:**
Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.).
Обратите внимание, что в базе данных есть несколько учителей с такой фамилией.

**Решение:**
```sql
SELECT
    sub.name subjects
FROM Subject sub
         JOIN Schedule sch on sch.subject = sub.id
         JOIN Teacher t on t.id = sch.teacher
WHERE t.last_name LIKE '%Romashkin%' and t.first_name LIKE 'P%' and t.middle_name LIKE 'P%'
```
</details>

<details>
<summary>Задание 41 - Начало четвёртого занятия</summary>

**Описание задачи:**
Выясните, во сколько по расписанию начинается четвёртое занятие.
Поля в результирующей таблице: start_pair

**Решение:**

```sql
SELECT t.start_pair
FROM Timepair t
WHERE EXISTS (
    SELECT 1
    FROM (
        SELECT
            id,
            start_pair,
            ROW_NUMBER() OVER (ORDER BY start_pair) as row_num
        FROM Timepair
    ) t2
    WHERE t.id = t2.id AND t2.row_num = 4
)
ORDER BY t.start_pair;
```
</details>

<details>
<summary>Задание 42 - Время, проведённое в школе</summary>

**Описание задачи:**
Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?
Используйте конструкцию "as time" для указания разницы во времени. Это необходимо для корректной проверки.
Результат должен быть в формате HH:MM:SS

**Решение:**
```sql
SELECT
  max(end_pair) - min(start_pair) time
FROM
  (
    SELECT
      DISTINCT ON (s.number_pair) s.number_pair,
      t.start_pair,
      t.end_pair
    FROM Schedule s
    JOIN Timepair t ON t.id = s.number_pair  AND t.id IN (2, 4)
    ORDER BY
      s.number_pair
  )

```
</details>

<details>
<summary>Задание 43 - Преподаватели физкультуры</summary>

**Описание задачи:**
Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке.
Поля в результирующей таблице:
last_name

**Решение:**
```sql
select t.last_name
from Teacher t
where exists(
    select 1
    from Schedule s
    join Subject sub on sub.id = s.subject
    where sub.name = 'Physical Culture'
    and t.id = s.teacher
)
order by t.last_name
```

</details>

<details>
<summary>Задание 44 - Максимальный возраст в 10 классах</summary>

**Описание задачи:**
Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW().
Используйте конструкцию "as max_year" для указания максимального возраста в годах. Это необходимо для корректной проверки.
Поля в результирующей таблице:
max_year

**Решение:**
```sql
SELECT EXTRACT(
		YEAR
		FROM AGE(NOW()::date, MIN(s.birthday::date))
	) AS max_year
FROM Student s
	JOIN Student_in_class sic ON sic.student = s.id
	JOIN Class c ON c.id = sic.class
	AND c.name LIKE '%10%'
```
```sql
SELECT EXTRACT(
		year
		FROM age(NOW(), s.birthday)
	) max_year
FROM Student s
	JOIN (
		SELECT c.name,
			sic.student
		FROM Class c
			JOIN Student_in_class sic ON sic.class = c.id
			AND c.name LIKE '%10%'
	) ss ON s.id = ss.student
ORDER BY s.birthday
LIMIT 1
```
</details>

<details>
<summary>Задание 45 - Самые используемые кабинеты</summary>

**Описание задачи:**
Какие кабинеты чаще всего использовались для проведения занятий?
Выведите те, которые использовались максимальное количество раз.
Поля в результирующей таблице:
classroom

**Решение:**
```sql
WITH classroom_counts AS (
    SELECT
        classroom,
        COUNT(*) AS usage_count
    FROM Schedule
    GROUP BY classroom
)
SELECT
    classroom
FROM classroom_counts
WHERE usage_count = (SELECT MAX(usage_count) FROM classroom_counts)
ORDER BY classroom;
```
</details>

<details>
<summary>Задание 46 - Классы преподавателя Krauze</summary>

**Описание задачи:**
В каких классах введет занятия преподаватель "Krauze" ?
Поля в результирующей таблице:
name

**Решение:**
```sql
SELECT DISTINCT
    c.name
FROM Schedule s
JOIN Class c ON c.id = s.class
JOIN Teacher t ON s.teacher = t.id and t.last_name = 'Krauze'
```
</details>

<details>
<summary>Задание 48 - Заполненность классов</summary>

**Описание задачи:**
Выведите заполненность классов в порядке убывания
Используйте конструкцию "as count" для агрегатной функции подсчета числа учащихся в классах. Это необходимо для корректной проверки.

**Решение:**
```sql
select
    c.name,
    count(*) as count
from Class c
join Student_in_class sic on sic.class = c.id
group by c.id
order by count desc
```
</details>

<details>
<summary>Задача 49 - Процент обучающихся в 10 A классе</summary>

**Описание задачи:**
Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201.
Используйте конструкцию "as percent" для представления результата вычисления. Это необходимо для корректной проверки.
Поля в результирующей таблице:
percent

**Решение:**
```sql
WITH students_10_a AS (
  SELECT
    COUNT(*) AS count_10_a
  FROM
    Student_in_class sic
    JOIN Class c ON c.id = sic.class
  WHERE
    c.name = '10 A'
),
total_students AS (
  SELECT
    COUNT(*) AS total_count
  FROM
    Student_in_class
)
SELECT
  ROUND(
    (
      s10.count_10_a * 100.0 / ts.total_count
    ):: numeric,
    4
  ) AS percent
FROM
  students_10_a s10,
  total_students ts;
```
```sql
WITH class_counts AS (
  SELECT
    COUNT(*) FILTER (
      WHERE
        c.name = '10 A'
    ) AS count_10_a,
    COUNT(*) AS total_count
  FROM
    Student_in_class sic
    JOIN Class c ON c.id = sic.class
)
SELECT
  ROUND(
    (count_10_a * 100.0 / total_count):: numeric,
    4
  ) AS percent
FROM
  class_counts;

```
</details>

<details>
<summary>Задание 50 - Процент родившихся в 2000 году</summary>

**Описание задачи:**
Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.
Используйте конструкцию "as percent" для указания процента. Это необходимо для корректной проверки.
Поля в результирующей таблице:
percent

**Решение:**
```sql
SELECT
    FLOOR(target_count * 100.0 / total_count) AS percent
FROM (
    SELECT
        COUNT(*) AS total_count,
        COUNT(*) FILTER(WHERE EXTRACT(YEAR FROM birthday) = 2000) AS target_count
    FROM Student
) AS counts;
```
</details>
