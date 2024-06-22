## Обобщенные табличные выражения

**Обобщенное табличное выражение** или **CTE** (Common Table Expressions) — это временная таблица, к которой можно обращаться в рамках **одного** запроса. Для простоты CTE можно считать именованным подзапросом, который определен отдельно от основного запроса. Использование CTE позволяет писать сложные запросы в более читаемой форме путем их разбиения на небольшие логические шаги.

Для написания запросов с использованием CTE предназначен оператор `WITH`, синтаксис которого имеет следующий вид:

```css
WITH <имя CTE> AS (
    <извлекающий запрос, определяющий содержимое CTE>
)

<основной запрос, который может обращаться к CTE>
```

Как видно из шаблона выше, для того чтобы воспользоваться CTE, ему необходимо дать имя, по которому к нему можно будет обращаться, а затем определить его содержимое. Обратите внимание, что определение CTE не отделяется от основного запроса точкой с запятой, поскольку вся конструкция (определение CTE и основной запрос) воспринимается как одно целое.

В качестве примера использования CTE напишем запрос, который среди всех книг за авторством `Stephen King` определяет ту, что была написана позже всех остальных.

Результатом приведенного ниже запроса:

```sql
WITH StephenKingBooks AS (
    SELECT title, release_year
    FROM Books INNER JOIN Authors ON Books.author_id = Authors.id
    WHERE Authors.name = 'Stephen' and Authors.surname = 'King' 
)

SELECT title
FROM StephenKingBooks
WHERE release_year = (SELECT MAX(release_year)
                      FROM StephenKingBooks);
```

является:

```no-highlight
+--------------+
| title        |
+--------------+
| The Outsider |
+--------------+
```

Запрос выше начинается с определения CTE с именем `StephenKingBooks`, которое извлекает из таблицы `Books` информацию обо всех книгах за авторством `Stephen King`:

```no-highlight
+-------------------+--------------+
| title             | release_year |
+-------------------+--------------+
| The Shining       | 1977         |
| The Green Mile    | 1996         |
| The Outsider      | 2018         |
| Dolores Claiborne | 1993         |
+-------------------+--------------+
```

Затем основной запрос извлекает из временной таблицы `StephenKingBooks` название той книги, которая имеет самый поздний год выхода, предварительно определяя этот год с помощью подзапроса. В данном примере CTE удобно тем, что позволяет вынести громоздкую операцию соединения таблиц из основного запроса, тем самым упростив его.

CTE является физической таблицей, которая перед выполнением основного запроса явно создается и помещается в оперативную память системы, пусть и на довольно небольшой промежуток времени.

В качестве дополнительного примера использования CTE напишем запрос, который среди всех книг за авторством `Stephen King` определяет ту, что была написана раньше всех остальных, а также ту, что была написана позже всех остальных.

Результатом приведенного ниже запроса:

```
WITH StephenKingBooks AS (
    SELECT title, release_year
    FROM Books INNER JOIN Authors ON Books.author_id = Authors.id
    WHERE Authors.name = 'Stephen' and Authors.surname = 'King' 
)

SELECT (SELECT title
        FROM StephenKingBooks
        WHERE release_year = (SELECT MIN(release_year)
                              FROM StephenKingBooks)) AS first_book,
       (SELECT title
        FROM StephenKingBooks
        WHERE release_year = (SELECT MAX(release_year)
                              FROM StephenKingBooks)) AS last_book;
```

является:

```no-highlight
+-------------+--------------+
| first_book  | last_book    |
+-------------+--------------+
| The Shining | The Outsider |
+-------------+--------------+
```

В данном примере снова используется предложенное ранее CTE `StephenKingBooks`. Сперва основной запрос извлекает из CTE название самой ранней книги, затем — самой поздней, при этом в обоих случаях для определения нужной книги используется подзапрос.

Одним из основных способов использования CTE является создание подзапросов, которые используются в рамках основного запроса несколько раз, что позволяет избежать повторного написания одних и тех же подзапросов.

После оператора `WITH` допустимо определить не только одно CTE, но и несколько, для этого достаточно перечислить их определения через запятую.

Результатом приведенного ниже запроса:

```sql
WITH StephenKingBooks AS (
    SELECT title, release_year
    FROM Books INNER JOIN Authors ON Books.author_id = Authors.id
    WHERE Authors.name = 'Stephen' and Authors.surname = 'King' 
),
JeromeSalingerBooks AS (
    SELECT title, release_year
    FROM Books INNER JOIN Authors ON Books.author_id = Authors.id
    WHERE Authors.name = 'Jerome' and Authors.surname = 'Salinger' 
)

SELECT *
FROM StephenKingBooks

UNION 

SELECT *
FROM JeromeSalingerBooks;
```

является:

```no-highlight
+------------------------+--------------+
| title                  | release_year |
+------------------------+--------------+
| The Shining            | 1977         |
| The Green Mile         | 1996         |
| The Outsider           | 2018         |
| Dolores Claiborne      | 1993         |
| The Catcher in the Rye | 1951         |
| Franny and Zooey       | 1961         |
+------------------------+--------------+
```

Здесь в блоке оператора `WITH` определяются два CTE, первое из которых содержит данные о книгах за авторством `Stephen King`, второе — за авторством `Jerome Salinger`. После основной запрос извлекает и объединяет содержимое обоих CTE.

Если блок `WITH` включает определение нескольких CTE, то их создание выполняется последовательно: сначала создается первое CTE, затем второе, и так далее. Таким образом, во время определения очередного CTE, все определенные ранее CTE уже являются созданными, поэтому каждое следующее определенное CTE может ссылаться на любое предыдущее.

Результатом приведенного ниже запроса:

```
WITH StephenKingBooks AS (
    SELECT title, release_year
    FROM Books INNER JOIN Authors ON Books.author_id = Authors.id
    WHERE Authors.name = 'Stephen' and Authors.surname = 'King'
),
StephenKingBooksReleasedIn1996 AS (
    SELECT title, release_year
    FROM StephenKingBooks
    WHERE release_year = 1996
)

SELECT *
FROM StephenKingBooksReleasedIn1996;
```

является:

```no-highlight
+----------------+--------------+
| title          | release_year |
+----------------+--------------+
| The Green Mile | 1996         |
+----------------+--------------+
```

Здесь происходит создание двух CTE, причем второе извлекает данные из первого.

CTE является обычной таблицей, поэтому с ней допустимо выполнять любые необходимые операции: извлекать данные, фильтровать и сортировать, объединять с другими таблицами, представлениями или CTE.

Несмотря на сходство представлений и CTE, между ними есть важное различие. Представление является реальным объектом базы данных — виртуальной таблицей, которая определяется единожды, а затем повсеместно используется при необходимости. CTE, в свою очередь, является временной таблицей, которая привязывается к одному конкретному запросу и существует лишь в рамках этого запроса.

  Представление является составляющей базы данных, CTE — составляющей одного запроса.

Также представление от CTE отличает то, что каждое обращение к представлению приводит к выполнению запроса, указанного при его определении. В случае с CTE такого не происходит, поскольку определение CTE влечет создание соответствующей таблицы, и все последующие операции с CTE выполняются как с обычной существующей таблицей.

 Важно помнить, что CTE существует только в рамках того запроса, в котором он определен.

Во время выполнения приведенного ниже запроса:

```
WITH StephenKingBooks AS (
    SELECT title, release_year
    FROM Books INNER JOIN Authors ON Books.author_id = Authors.id
    WHERE Authors.name = 'Stephen' and Authors.surname = 'King'
)

SELECT *
FROM StephenKingBooks
LIMIT 1;

SELECT *
FROM StephenKingBooks
LIMIT 2;
```

второй извлекающий запрос завершится с ошибкой:

```no-highlight
ERROR 1146: Table 'StephenKingBooks' doesn't exist
```

 Определить названия полей CTE можно не только в самом запросе с помощью псевдонимов, но и путем указания этих имен в скобках через запятую после названия CTE.

Результатом приведенного ниже запроса:

```
WITH FirstBooks (bookname, year) AS (
    SELECT title, release_year
    FROM Books
    LIMIT 3
)

SELECT *
FROM FirstBooks;
```

является:

```no-highlight
+------------------------+------+
| bookname               | year |
+------------------------+------+
| The Shining            | 1977 |
| Fight Club             | 1996 |
| The Catcher in the Rye | 1951 |
+------------------------+------+
```