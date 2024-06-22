Если необходимо получить информацию о полях таблицы, можно воспользоваться оператором `DESCRIBE`.

Результатом приведенного ниже запроса:

```sql
CREATE TABLE Authors
(
    id      INT PRIMARY KEY AUTO_INCREMENT,
    name    VARCHAR(40),
    surname VARCHAR(40)
);

CREATE TABLE Books
(
    id        INT PRIMARY KEY AUTO_INCREMENT,
    title     VARCHAR(40),
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES Authors (id)
);

DESCRIBE Books;
```

является:

```no-highlight
+-----------+-------------+------+-----+---------+----------------+
| Field     | Type        | Null | Key | Default | Extra          |
+-----------+-------------+------+-----+---------+----------------+
| id        | int         | NO   | PRI | NULL    | auto_increment |
| title     | varchar(40) | YES  |     | NULL    |                |
| author_id | int         | YES  | MUL | NULL    |                |
+-----------+-------------+------+-----+---------+----------------+
```
#sql
