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

В SQL левое внешнее соединение выполняется с помощью оператора `LEFT OUTER JOIN` и ключевого слова `ON`. Сначала указывается первая таблица, затем оператор `LEFT OUTER JOIN`, а после вторая таблица. Завершается выражение ключевым словом `ON`, после которого располагается условие соединения.

Общий синтаксис левого внешнего соединения имеет следующий вид:

```css
<первая таблица> LEFT OUTER JOIN <вторая таблица> ON <условие соединения>
```

В качестве примера выполним левое внешнее соединение таблиц `Books` и `Authors` с условием `Books.author_id = Authors.id`.

Результатом приведенного ниже запроса:

```
SELECT *
FROM Books LEFT OUTER JOIN Authors ON Books.author_id = Authors.id;
```

является:

```no-highlight
+----+-----------+-----------------------+------+-----------------+
| id | author_id | title                 | id   | author          |
+----+-----------+-----------------------+------+-----------------+
| 1  | NULL      | Old Gods of Asgard    | NULL | NULL            |
| 2  | 1         | Fight Club            | 1    | Leo Tolstoy     |
| 3  | 2         | The Green Mile        | 2    | Chuck Palahniuk |
| 4  | 3         | The Lord of the Rings | 3    | Stephen King    |
| 5  | 2         | It                    | 2    | Chuck Palahniuk |
| 6  | 1         | Haunted               | 1    | Leo Tolstoy     |
| 7  | 2         | The Shining           | 2    | Chuck Palahniuk |
| 8  | NULL      | Friendly Neighborhood | NULL | NULL            |
| 9  | 2         | Pet Sematary          | 2    | Chuck Palahniuk |
| 10 | NULL      | Homesick              | NULL | NULL            |
+----+-----------+-----------------------+------+-----------------+
```

Таблица, полученная в результате данного соединения, сопоставляет каждой книге ее автора, причем, если автор книги неизвестен, значением поля `author` является `NULL`. Используя эту таблицу, мы можем извлечь названия всех книг с указанием авторства каждой. Если авторство неизвестно, укажем строку `Unknown`.

Результатом приведенного ниже запроса:

```
SELECT title,
       IFNULL(author, 'Unknown') AS author
FROM Books LEFT OUTER JOIN Authors ON Books.author_id = Authors.id;
```

является:

```no-highlight
+-----------------------+-----------------+
| title                 | author          |
+-----------------------+-----------------+
| Old Gods of Asgard    | Unknown         |
| Fight Club            | Leo Tolstoy     |
| The Green Mile        | Chuck Palahniuk |
| The Lord of the Rings | Stephen King    |
| It                    | Chuck Palahniuk |
| Haunted               | Leo Tolstoy     |
| The Shining           | Chuck Palahniuk |
| Friendly Neighborhood | Unknown         |
| Pet Sematary          | Chuck Palahniuk |
| Homesick              | Unknown         |
+-----------------------+-----------------+
```