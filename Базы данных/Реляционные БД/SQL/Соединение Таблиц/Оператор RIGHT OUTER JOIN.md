**Первая таблица.** Информация об авторах книг располагается в таблице `Authors`, которая имеет следующий вид:

```no-highlight
+----+-----------------+
| id | author          |
+----+-----------------+
| 1  | Leo Tolstoy     |
| 2  | Chuck Palahniuk |
| 3  | Stephen King    |
| 4  | John Tolkien    |
| 5  | Joanne Rowling  |
+----+-----------------+
```

Первое поле этой таблицы содержит идентификатор автора, второе — имя и фамилию.

**Вторая таблица.** Информация о размещенных в библиотеке книгах располагается в таблице `Books`, которая имеет следующий вид:

```no-highlight
+----+-----------+-----------------------+
| id | author_id | title                 |
+----+-----------+-----------------------+
| 1  | NULL      | Old Gods of Asgard    |
| 2  | 1         | Fight Club            |
| 3  | 2         | The Green Mile        |
| 4  | 3         | The Lord of the Rings |
| 5  | 2         | It                    |
| 6  | 1         | Haunted               |
| 7  | 2         | The Shining           |
| 8  | NULL      | Friendly Neighborhood |
| 9  | 2         | Pet Sematary          |
| 10 | NULL      | Homesick              |
+----+-----------+-----------------------+
```

Первое поле этой таблицы содержит идентификатор книги, второе — идентификатор автора, третье — название книги. Если автор книги неизвестен, значением поля `author_id` является `NULL`.

Схема базы данных

В SQL правое внешнее соединение выполняется с помощью оператора `RIGHT OUTER JOIN` и ключевого слова `ON`. Сначала указывается первая таблица, затем оператор `RIGHT OUTER JOIN`, а после вторая таблица. Завершается выражение ключевым словом `ON`, после которого располагается условие соединения.

Общий синтаксис правого внешнего соединения имеет следующий вид:

```css
<первая таблица> RIGHT OUTER JOIN <вторая таблица> ON <условие соединения>
```

В качестве примера выполним правое внешнее соединение таблиц `Books` и `Authors` с условием `Books.author_id = Authors.id`.

Результатом приведенного ниже запроса:

```
SELECT *
FROM Books RIGHT OUTER JOIN Authors ON Books.author_id = Authors.id;
```

является:

```no-highlight
+------+-----------+-----------------------+----+-----------------+
| id   | author_id | title                 | id | author          |
+------+-----------+-----------------------+----+-----------------+
| 6    | 1         | Haunted               | 1  | Leo Tolstoy     |
| 2    | 1         | Fight Club            | 1  | Leo Tolstoy     |
| 9    | 2         | Pet Sematary          | 2  | Chuck Palahniuk |
| 7    | 2         | The Shining           | 2  | Chuck Palahniuk |
| 5    | 2         | It                    | 2  | Chuck Palahniuk |
| 3    | 2         | The Green Mile        | 2  | Chuck Palahniuk |
| 4    | 3         | The Lord of the Rings | 3  | Stephen King    |
| NULL | NULL      | NULL                  | 4  | John Tolkien    |
| NULL | NULL      | NULL                  | 5  | Joanne Rowling  |
+------+-----------+-----------------------+----+-----------------+
```

Используя таблицу, полученную в результате приведенного выше соединения, мы можем определить, сколько книг принадлежит каждому автору. Для этого достаточно сгруппировать записи по авторам и посчитать количество существующих книг у каждого из них.

Результатом приведенного ниже запроса:

```sql
SELECT author,
       COUNT(Books.id) AS books
FROM Books RIGHT OUTER JOIN Authors ON Books.author_id = Authors.id
GROUP BY author;
```

является:

```no-highlight
+-----------------+-------+
| author          | books |
+-----------------+-------+
| Leo Tolstoy     | 2     |
| Chuck Palahniuk | 4     |
| Stephen King    | 1     |
| John Tolkien    | 0     |
| Joanne Rowling  | 0     |
+-----------------+-------+
```