# Решения заданий тренажёра от [sql-academy](https://sql-academy.org)

<details>
<summary>Задание 1</summary>

**Описание задачи:**
Вывести имена всех людей, которые есть в базе данных авиакомпании.

**Решение:**
```sql
SELECT name FROM Passenger;
```
</details>

<details>
<summary>Задание 2</summary>

**Описание задачи:**
Вывести названия всеx авиакомпаний

**Решение:**
```sql
SELECT c.name
FROM Company c
```
</details>


<details>
<summary>Задание 3</summary>

**Описание задачи:**
Вывести все рейсы, совершенные из Москвы


**Решение:**
```sql
SELECT *
FROM Trip t
WHERE t.town_from = 'Moscow'
```
</details>



<details>
<summary>Задание 4</summary>

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
<summary>Задание 5</summary>

**Описание задачи:**
Вывести имена людей, которые заканчиваются на "man"

**Решение:**
```sql
SELECT name
FROM Passenger p
WHERE p.name LIKE '%man%'
```
</details>

<details>
<summary>Задание 5</summary>

**Описание задачи:**
Вывести количество рейсов, совершенных на TU-134

**Решение:**
```sql
SELECT COUNT(t.id) as count
FROM Trip t
WHERE t.plane = 'TU-134'
```
</details>


<details>
<summary>Задание 6</summary>

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
<summary>Задание 7</summary>

**Описание задачи:**

**Решение:**
```sql
SELECT DISTINCT
    t.plane
FROM Trip t
WHERE t.town_to='Moscow'
```
</details>


<details>
<summary>Задание 8</summary>

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
<summary>Задание 9</summary>

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
<summary>Задание 10</summary>

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
<summary>Задание 11</summary>
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
<summary>Задание 12</summary>

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
<summary>Задание 13</summary>

**Описание задачи:**

**Решение:**
```sql

```
</details>

<details>
<summary>Задание 14</summary>

**Описание задачи:**

**Решение:**
```sql

```
</details>


<details>
<summary>Задание 15</summary>

**Описание задачи:**

**Решение:**
```sql

```
</details>