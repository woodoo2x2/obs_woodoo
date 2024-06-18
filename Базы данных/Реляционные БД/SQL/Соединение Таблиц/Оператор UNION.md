**Первая таблица.** Информация о студентах располагается в таблице `Students`, которая имеет следующий вид:

```no-highlight
+----+-------+---------+-----+
| id | name  | surname | age |
+----+-------+---------+-----+
| 1  | Peter | Parker  | 18  |
| 2  | Gwen  | Stacy   | 21  |
| 3  | Flash | Tompson | 19  |
| 4  | Mary  | Jane    | 18  |
| 5  | Eddie | Brock   | 21  |
+----+-------+---------+-----+
```

Первое поле этой таблицы содержит идентификатор студента, второе — имя, третье — фамилию, четвертое — возраст в годах.

**Вторая таблица.** Информация о преподавателях располагается в таблице `Teachers`, которая имеет следующий вид:

```no-highlight
+----+---------+---------+-------------+
| id | name    | surname | subject     |
+----+---------+---------+-------------+
| 1  | Norman  | Osborn  | algebra     |
| 2  | Curt    | Connors | informatics |
| 3  | Harry   | Osborn  | english     |
| 4  | Flint   | Marko   | chemistry   |
| 5  | Richard | Parker  | physics     |
+----+---------+---------+-------------+
```

Первое поле этой таблицы содержит идентификатор преподавателя, второе — имя, третье — фамилию, четвертое — название преподаваемого предмета.


## Оператор UNION

Одной из возможностей SQL является объединение нескольких результатов запросов в один результирующий набор. Для этого используется оператор `UNION`, который указывается между запросами, результаты которых необходимо объединить.

В качестве примера извлечем из таблицы `Students` идентификаторы, имена и фамилии всех студентов, а из таблицы `Teachers` — идентификаторы, имена и фамилии всех учителей, а затем объединим полученные результаты.

Результатом приведенного ниже запроса:

```
SELECT id, name, surname
FROM Students

UNION

SELECT id, name, surname
FROM Teachers;
```

является:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Peter   | Parker  |
| 2  | Gwen    | Stacy   |
| 3  | Flash   | Tompson |
| 4  | Mary    | Jane    |
| 5  | Eddie   | Brock   |
| 1  | Norman  | Osborn  |
| 2  | Curt    | Connors |
| 3  | Harry   | Osborn  |
| 4  | Flint   | Marko   |
| 5  | Richard | Parker  |
+----+---------+---------+
```

Как видно из примера, результирующая таблица включает в себя результат первого запроса (данные всех студентов), к которому добавлен результат второго запроса (данные всех учителей).

 Запрос, который объединяет результаты выполнения нескольких запросов, называют **комбинированным**.

Для того чтобы объединить результаты произвольного количества запросов, а не только двух, необходимо указать оператор `UNION` между всеми запросами, результаты которых требуют объединения.

Результатом приведенного ниже запроса:

```
SELECT id, name, surname
FROM Students
WHERE id = 1

UNION

SELECT id, name, surname
FROM Students
WHERE id = 2

UNION

SELECT id, name, surname
FROM Students
WHERE id = 3;
```

является:

```no-highlight
+----+-------+---------+
| id | name  | surname |
+----+-------+---------+
| 1  | Peter | Parker  |
| 2  | Gwen  | Stacy   |
| 3  | Flash | Tompson |
+----+-------+---------+
```

В данном примере каждый из запросов извлекает по одной записи из таблицы `Students`. Первый запрос извлекает первую запись, второй — вторую, третий — третью, затем полученные записи объединяются в одну результирующую таблицу.

При использовании оператора `UNION` объединяемые результаты запросов обязательно должны иметь одинаковое количество полей, в противном случае произойдет ошибка.

Результатом приведенного ниже запроса:

```
SELECT name, surname        -- два поля
FROM Students

UNION

SELECT id, name, surname    -- три поля
FROM Teachers;
```

является ошибка:

```no-highlight
ERROR 1222: The used SELECT statements have a different number of columns
```

Названия полей в объединяемых результатах запросов необязательно должны быть одинаковыми, поскольку в качестве итоговых названий полей используются названия самого первого результата запроса.

Результатом приведенного ниже запроса:

```
SELECT name AS firstname, surname AS lastname
FROM Students

UNION

SELECT name, surname
FROM Teachers;
```

является:

```no-highlight
+-----------+----------+
| firstname | lastname |
+-----------+----------+
| Peter     | Parker   |
| Gwen      | Stacy    |
| Flash     | Tompson  |
| Mary      | Jane     |
| Eddie     | Brock    |
| Norman    | Osborn   |
| Curt      | Connors  |
| Harry     | Osborn   |
| Flint     | Marko    |
| Maxwell   | Dillon   |
+-----------+----------+
```

Типы значений в соответствующих полях объединяемых результатов также могут быть различными.

Результатом приведенного ниже запроса:

```
SELECT name, surname, age
FROM Students

UNION

SELECT name, surname, subject
FROM Teachers;
```

является:

```no-highlight
+---------+---------+-------------+
| name    | surname | age         |
+---------+---------+-------------+
| Peter   | Parker  | 18          |
| Gwen    | Stacy   | 21          |
| Flash   | Tompson | 27          |
| Mary    | Jane    | 19          |
| Eddie   | Brock   | 26          |
| Norman  | Osborn  | Algebra     |
| Curt    | Connors | Informatics |
| Harry   | Osborn  | English     |
| Flint   | Marko   | Chemistry   |
| Richard | Parker  | Physics     |
+---------+---------+-------------+
```

Здесь результат первого запроса в третьем поле содержит числовые значения, в то время как результат второго запроса в том же поле содержит строковые значения.

Несмотря на то, что единственным ограничением при объединении результатов нескольких запросов является одинаковое количество полей, объединять рекомендуется только однотипные результаты.

### Ключевые слова DISTINCT и ALL

После оператора `UNION` может быть указано одно из ключевых слов [[Оператор DISTINCT (Извлечение уникальных записей)]] или `ALL`. Если указано ключевое слово `DISTINCT`, то после объединения результатов запросов будет выполнено дополнительное удаление одинаковых записей.

Например, в наших таблицах имеются студенты и преподаватели с одинаковыми фамилиями: два преподавателя с фамилией `Osborn`, студент и преподаватель с фамилией `Parker`. Если мы извлечем фамилии всех студентов и преподавателей, а затем объединим полученные результаты с помощью связки `UNION DISTINCT`, то получим лишь уникальные значения.

Результатом приведенного ниже запроса:

```
SELECT surname
FROM Students

UNION DISTINCT

SELECT surname
FROM Teachers;
```

является:

```no-highlight
+---------+
| surname |
+---------+
| Parker  |
| Stacy   |
| Tompson |
| Jane    |
| Brock   |
| Osborn  |
| Connors |
| Marko   |
+---------+
```

Как видно по результирующей таблице, повторяющиеся фамилии `Parker` и `Osborn` были исключены.

Если за оператором `UNION` следует ключевое слово `ALL`, то после объединения результатов запросов дополнительное удаление одинаковых записей выполнено не будет.

Результатом приведенного ниже запроса:

```
SELECT surname
FROM Students

UNION ALL

SELECT surname
FROM Teachers;
```

является (дубликаты помечены соответствующими цветами):

```no-highlight
+---------+
| surname |
+---------+
| Parker  |
| Stacy   |
| Tompson |
| Jane    |
| Brock   |
| Osborn  |
| Connors |
| Osborn  |
| Marko   |
| Parker  |
+---------+
```

По умолчанию оператор `UNION` выполняет удаление одинаковых записей, поэтому ключевое слово `DISTINCT` используется лишь для большей наглядности.

Результатом приведенного ниже запроса:

```
SELECT surname
FROM Students

UNION

SELECT surname
FROM Teachers;
```

является:

```no-highlight
+---------+
| surname |
+---------+
| Parker  |
| Stacy   |
| Tompson |
| Jane    |
| Brock   |
| Osborn  |
| Connors |
| Marko   |
+---------+
```

### Операторы LIMIT и ORDER BY

Чтобы воспользоваться оператором [[Операторы LIMIT и OFFSET (ограничение результатов запроса)|LIMIT]] в одном или нескольких запросах, расположенных между оператором `UNION`, эти запросы необходимо заключить в круглые скобки.

Результатом приведенного ниже запроса:

```
(SELECT id, name, surname
FROM Students
LIMIT 1)

UNION

(SELECT id, name, surname
FROM Teachers
LIMIT 1);
```

является:

```no-highlight
+----+--------+---------+
| id | name   | surname |
+----+--------+---------+
| 1  | Peter  | Parker  |
| 1  | Norman | Osborn  |
+----+--------+---------+
```

В данном примере с помощью оператора `LIMIT` первый запрос извлекает одну запись из таблицы `Students`, второй запрос — одну запись из таблицы `Teachers`, поэтому оба запроса заключены в круглые скобки.

Аналогичным образом необходимо заключать в скобки те запросы, в которых используется оператор [[Оператор ORDER BY|ORDER BY]].

Результатом приведенного ниже запроса:

```
(SELECT id, name, surname
FROM Students
ORDER BY id)

UNION

(SELECT id, name, surname
FROM Teachers
ORDER BY id);
```

является:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Peter   | Parker  |
| 2  | Gwen    | Stacy   |
| 3  | Flash   | Tompson |
| 4  | Mary    | Jane    |
| 5  | Eddie   | Brock   |
| 1  | Norman  | Osborn  |
| 2  | Curt    | Connors |
| 3  | Harry   | Osborn  |
| 4  | Flint   | Marko   |
| 5  | Richard | Parker  |
+----+---------+---------+
```

Следует заметить, что оператор `ORDER BY` в отдельных запросах на самом деле не определяет порядок, в котором записи будут располагаться в результирующей таблице.

Результатом приведенного ниже запроса:

```
(SELECT id, name, surname
FROM Students
ORDER BY id DESC)

UNION

(SELECT id, name, surname
FROM Teachers
ORDER BY id DESC);
```

является:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Peter   | Parker  |
| 2  | Gwen    | Stacy   |
| 3  | Flash   | Tompson |
| 4  | Mary    | Jane    |
| 5  | Eddie   | Brock   |
| 1  | Norman  | Osborn  |
| 2  | Curt    | Connors |
| 3  | Harry   | Osborn  |
| 4  | Flint   | Marko   |
| 5  | Richard | Parker  |
+----+---------+---------+
```

В примере выше записи в обеих таблицах перед объединением сортируются по убыванию значения поля `id`, однако в результирующей таблице они располагаются в порядке возрастания значения поля `id`.

Происходит это по той причине, что оператор `UNION`, объединяя результаты нескольких запросов, всего лишь создает неупорядоченный набор записей и не гарантирует, что записи сохранят тот порядок, в котором они в этот набор были добавлены.

Несмотря на это, оператор `ORDER BY` в отдельных запросах может быть полезен в связке с оператором `LIMIT` для выполнения более точной выборки.

Результатом приведенного ниже запроса:

```
(SELECT id, name, surname
FROM Students
ORDER BY id DESC LIMIT 1)

UNION

SELECT id, name, surname
FROM Teachers
WHERE id = 5;
```

является:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 5  | Eddie   | Brock   |
| 5  | Richard | Parker  |
+----+---------+---------+
```

Здесь оба запроса извлекают из соответствующих таблиц последнюю запись, однако первый запрос делает это с помощью сочетания операторов `ORDER BY` и `LIMIT`, а второй с помощью — оператора `WHERE`.

Если оператор `ORDER BY` появляется в одном или нескольких объединяемых запросах без оператора `LIMIT`, то в целях оптимизации сортировка даже не выполняется.

Хоть с помощью оператора `ORDER BY` и нельзя выполнить сортировку каждого отдельного результата запроса, с помощью него можно отсортировать полученную после объединения всех результатов таблицу. Для этого нужно единожды указать оператор `ORDER BY` после самого последнего объединяемого запроса.

Результатом приведенного ниже запроса:

```
SELECT id, name, surname
FROM Students

UNION

SELECT id, name, surname
FROM Teachers
ORDER BY id;
```

является:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Peter   | Parker  |
| 1  | Norman  | Osborn  |
| 2  | Gwen    | Stacy   |
| 2  | Curt    | Connors |
| 3  | Flash   | Tompson |
| 3  | Harry   | Osborn  |
| 4  | Mary    | Jane    |
| 4  | Flint   | Marko   |
| 5  | Eddie   | Brock   |
| 5  | Richard | Parker  |
+----+---------+---------+
```

В примере выше сперва выполняется объединение результатов обоих запросов, а затем сортировка по значению поля `id`. При выполнении подобной сортировки каждый объединяемый запрос можно заключать в круглые скобки для большей наглядности.

Результатом приведенного ниже запроса:

```
(SELECT id, name, surname
FROM Students)

UNION

(SELECT id, name, surname
FROM Teachers)
ORDER BY id;
```

является:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Peter   | Parker  |
| 1  | Norman  | Osborn  |
| 2  | Gwen    | Stacy   |
| 2  | Curt    | Connors |
| 3  | Flash   | Tompson |
| 3  | Harry   | Osborn  |
| 4  | Mary    | Jane    |
| 4  | Flint   | Marko   |
| 5  | Eddie   | Brock   |
| 5  | Richard | Parker  |
+----+---------+---------+
```

Аналогичным образом к полученной после объединения всех результатов таблице можно применить оператор `LIMIT` или связку операторов `ORDER BY` и `LIMIT`.

При объединении результатов нескольких запросов для обозначения того, какой таблице принадлежит каждая запись, можно использовать вспомогательное поле.

Результатом приведенного ниже запроса:

```
SELECT name, surname, 'Students' AS from_table
FROM Students

UNION

SELECT name, surname, 'Teachers'
FROM Teachers;
```

является:

```no-highlight
+---------+---------+------------+
| name    | surname | from_table |
+---------+---------+------------+
| Peter   | Parker  | Students   |
| Gwen    | Stacy   | Students   |
| Flash   | Tompson | Students   |
| Mary    | Jane    | Students   |
| Eddie   | Brock   | Students   |
| Norman  | Osborn  | Teachers   |
| Curt    | Connors | Teachers   |
| Harry   | Osborn  | Teachers   |
| Flint   | Marko   | Teachers   |
| Richard | Parker  | Teachers   |
+---------+---------+------------+
```

Также вспомогательное поле может использоваться при необходимости гарантировать то, что в полученной после объединения результатов таблице сначала будут следовать записи одного результата, а затем — другого.

Результатом приведенного ниже запроса:

```
(SELECT name, surname, 1 AS sortvalue
FROM Students)

UNION

(SELECT name, surname, 2
FROM Teachers)
ORDER BY sortvalue;
```

является:

```no-highlight
+---------+---------+-----------+
| name    | surname | sortvalue |
+---------+---------+-----------+
| Peter   | Parker  | 1         |
| Gwen    | Stacy   | 1         |
| Flash   | Tompson | 1         |
| Mary    | Jane    | 1         |
| Eddie   | Brock   | 1         |
| Norman  | Osborn  | 2         |
| Curt    | Connors | 2         |
| Harry   | Osborn  | 2         |
| Flint   | Marko   | 2         |
| Richard | Parker  | 2         |
+---------+---------+-----------+
```

Здесь итоговая сортировка выполняется по значению поля `sortvalue`. Данная сортировка гарантирует то, что никакая запись из таблицы `Teachers` не встретится раньше записи из таблицы `Students`.

 Таблица, получаемая после объединения результатов нескольких запросов, может быть указана в блоке оператора `FROM` для последующей работы с ней.

Результатом приведенного ниже запроса:

```
SELECT name, surname
FROM (SELECT * FROM Students
      
      UNION
      
      SELECT * FROM Teachers) AS StudentsTeachers;
```

является:

```no-highlight
+---------+---------+
| name    | surname |
+---------+---------+
| Peter   | Parker  |
| Gwen    | Stacy   |
| Flash   | Tompson |
| Mary    | Jane    |
| Eddie   | Brock   |
| Norman  | Osborn  |
| Curt    | Connors |
| Harry   | Osborn  |
| Flint   | Marko   |
| Richard | Parker  |
+---------+---------+
```

Обратите внимание, что при таком сценарии использования объединяемые запросы необходимо заключить в круглые скобки, а также дать результату объединения псевдоним.