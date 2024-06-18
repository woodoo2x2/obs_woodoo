## Внешний ключ

Одним из самых важных ограничений является `FOREIGN KEY`, которое предназначено для определения поля как внешнего ключа. Помимо построения связей между таблицами, оно выполняет постоянную поддержку согласованности этих связей. Если пренебрегать ограничением `FOREIGN KEY`, то в процессе работы с таблицами базы данных можно прийти к ситуации, в которой согласованность связанных данных будет нарушена, что, например, может означать то, что какая-либо таблица будет ссылаться на несуществующие данные.

В качестве примера использования ограничения `FOREIGN KEY` создадим две связанные таблицы. Первой из них будет таблица `Authors`, предназначенная для хранения информации о различных авторах книг. Она будет включать три следующих поля:

- `id` — целочисленное поле, содержащее уникальный непустой идентификатор автора
- `name` — строковое поле, содержащее имя автора
- `surname` — строковое поле, содержащее фамилию автора

Второй таблицей будет предложенная в начале урока таблица `Books`, но с некоторыми изменениями. Она будет включать три следующих поля:

- `id` — целочисленное поле, содержащее уникальный непустой идентификатор книги
- `title` — строковое поле, содержащее название книги
- `author_id` — целочисленное поле, содержащее идентификатор автора

Связь между таблицами строится посредством внешнего ключа `author_id` таблицы `Books`, которое ссылается на поле `id` таблицы `Authors`. Таким образом, таблица `Authors` является родительской, а таблица `Books` — дочерней.

Теперь рассмотрим запрос, создающий предложенные таблицы. Он выглядит следующим образом:

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
```

Часть запроса, создающая таблицу `Authors`, полностью знакома, поэтому сразу перейдем к той части, в которой происходит создание таблицы `Books`, а точнее определение ее внешнего ключа. Оно начинается с ограничения `FOREIGN KEY`, после которого в скобках указывается поле, являющееся внешним ключом. В нашем случае это поле `author_id`. Затем следует ключевое слово `REFERENCES`, а после него — имя родительской таблицы и вновь в скобках поле этой таблицы, на которое ссылается внешний ключ. В нашем случае внешний ключ ссылается на поле `id` таблицы `Authors`.

Внешний ключ и поле, на которое оно ссылается, должны иметь одинаковые типы данных. Размер и знак типов с фиксированной точностью, таких как `INTEGER` и `DECIMAL`, должны быть одинаковыми.

Поле, являющееся внешним ключом, обладает рядом соответствующих особенностей. Например, внешний ключ не может принимать значения, которых нет в поле, на которое он ссылается.

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

INSERT INTO Authors (name, surname)
VALUES ('Stephen', 'King'),
       ('Joseph', 'Conrad');
       
INSERT INTO Books (title, author_id)
VALUES ('War and Peace', 3);
```

является ошибка:

```no-highlight
ERROR 1452: Cannot add or update a child row: a foreign key constraint fails (`Books`, CONSTRAINT `Books_ibfk_1` FOREIGN KEY (`author_id`) REFERENCES `Authors` (`author_id`))
```

Запрос выше сначала добавляет в таблицу `Authors` две записи:

```no-highlight
​+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Stephen | King    |
| 2  | Joseph  | Conrad  |
+----+---------+---------+
```

Затем выполняется попытка добавить в таблицу `Books` запись, значение поля `author_id` которой равняется `3`. Но поле `id` таблицы `Authors`, на которое ссылается поле `author_id` таблицы `Books`, не содержит значения `3`, поэтому и само поле `author_id` не может содержать данное значение.

### Поведение при обновлении и удалении

Когда выполняется попытка изменить запись в родительской таблице (а именно значение ее поля, связанного с внешним ключом), от которой зависят записи в дочерней таблице, происходит ошибка.

Результатом приведенного ниже запроса:

```sql
CREATE TABLE Authors
(
    id      INT PRIMARY KEY AUTO_INCREMENT,
    name    VARCHAR(40),
    surname VARCHAR(40)
);

CREATE TABLE Books (
    id        INT PRIMARY KEY AUTO_INCREMENT,
    title     VARCHAR(40),
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES Authors (id)
);

INSERT INTO Authors (name, surname)
VALUES ('Stephen', 'King'),
       ('Joseph', 'Conrad');
       
INSERT INTO Books (title, author_id)
VALUES ('It', 1);

UPDATE Authors
SET id = 3
WHERE id = 1;
```

является ошибка:

```no-highlight
ERROR 1451: Cannot delete or update a parent row: a foreign key constraint fails (`beegeek`.`books`, CONSTRAINT `books_ibfk_1` FOREIGN KEY (`author_id`) REFERENCES `authors` (`id`))
```

В данном запросе сначала в таблицу `Authors` добавляются две записи:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Stephen | King    |
| 2  | Joseph  | Conrad  |
+----+---------+---------+
```

Затем в таблицу `Books` добавляется одна запись :

```no-highlight
+----+-------+-----------+
| id | title | author_id |
+----+-------+-----------+
| 1  | It    | 1         |
+----+-------+-----------+
```

Первая запись таблицы `Books` становится зависимой от первой записи таблицы `Authors`, поэтому последующая попытка изменить значение поля `id` первой записи таблицы `Authors` с `1` на `3` приводит к ошибке. Аналогичная ошибка возникнет и в том случае, если произойдет попытка удалить первую запись таблицы `Authors`.

Поведение при обновлении или удалении связанных данных является настраиваемым. Чтобы определить действие, которое должно быть выполнено при обновлении, нужно после определения внешнего ключа указать дополнительный оператор `ON UPDATE`, а чтобы определить действие, которое должно быть выполнено при удалении, — оператор `ON DELETE`.

Само же действие определяется одним из трех ключевых слов: `CASCADE, SET NULL` или `RESTRICT`. Оно указывается сразу после оператора `ON UPDATE` (`ON DELETE`) и задает соответствующее поведение при обновлении (удалении) связанных данных.

При использовании ключевого слова `RESTRICT` изменение или удаление связанных данных приводит к ошибке. Другими словами, данное ключевое слово задает поведение по умолчанию.

Если используется ключевое слово `CASCADE`, то при изменении или удалении данных в родительской таблице аналогичные действия будут автоматически применены и к связанным записям в дочерней таблице.

После выполнения приведенного ниже запроса:

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
        ON UPDATE CASCADE
        ON DELETE CASCADE
);

INSERT INTO Authors (name, surname)
VALUES ('Stephen', 'King'),
       ('Joseph', 'Conrad');
       
INSERT INTO Books (title, author_id)
VALUES ('It', 1),
       ('Heart of Darkness', 2),
       ('Pet Sematary', 1);

UPDATE Authors
SET id = 3
WHERE id = 1;
```

таблица `Books` будет иметь вид:

```no-highlight
+----+-------------------+-----------+
| id | title             | author_id |
+----+-------------------+-----------+
| 1  | It                | 3         |
| 2  | Heart of Darkness | 2         |
| 3  | Pet Sematary      | 3         |
+----+-------------------+-----------+
```

таблица `Authors` будет иметь вид:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 2  | Joseph  | Conrad  |
| 3  | Stephen | King    |
+----+---------+---------+
```

Запрос выше сначала добавляет в таблицу `Authors` две записи:

```no-highlight
+----+---------+---------+
| id | name    | surname |
+----+---------+---------+
| 1  | Stephen | King    |
| 2  | Joseph  | Conrad  |
+----+---------+---------+
```

После добавляются три записи в таблицу `Books`:

```no-highlight
+----+-------------------+-----------+
| id | title             | author_id |
+----+-------------------+-----------+
| 1  | It                | 1         |
| 2  | Heart of Darkness | 2         |
| 3  | Pet Sematary      | 1         |
+----+-------------------+-----------+
```

Первая и третья записи таблицы `Books` зависимы от первой записи таблицы `Authors`, однако последующее изменение значения поля `id` первой записи таблицы `Authors` с `1` на `3` приводит не к ошибке, а к аналогичному изменению значений поля `author_id` первой и третьей записей таблицы `Books`.

После выполнения приведенного ниже запроса:

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
        ON UPDATE CASCADE
        ON DELETE CASCADE
);

INSERT INTO Authors (name, surname)
VALUES ('Stephen', 'King'),
       ('Joseph', 'Conrad');
       
INSERT INTO Books (title, author_id)
VALUES ('It', 1),
       ('Heart of Darkness', 2),
       ('Pet Sematary', 1);

DELETE FROM Authors
WHERE id = 1;
```

таблица `Books` будет иметь вид:

```no-highlight
+----+-------------------+-----------+
| id | title             | author_id |
+----+-------------------+-----------+
| 2  | Heart of Darkness | 2         |
+----+-------------------+-----------+
```

таблица `Authors` будет иметь вид:

```no-highlight
+----+--------+---------+
| id | name   | surname |
+----+--------+---------+
| 2  | Joseph | Conrad  |
+----+--------+---------+
```

Данный запрос практически полностью повторяет предыдущий запрос. Разница заключается в том, что здесь запись таблицы `Authors` не изменяется, а удаляется, поэтому и зависимые записи таблицы `Books` также удаляются.

Если используется ключевое слово `SET NULL`, то при изменении или удалении данных в родительской таблице связанные записи в дочерней таблице в качестве значения внешнего ключа примут значение `NULL`.

После выполнения приведенного ниже запроса:

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
        ON UPDATE SET NULL
        ON DELETE SET NULL
);

INSERT INTO Authors (name, surname)
VALUES ('Stephen', 'King'),
       ('Joseph', 'Conrad');
       
INSERT INTO Books (title, author_id)
VALUES ('It', 1),
       ('Heart of Darkness', 2),
       ('Pet Sematary', 1);

UPDATE Authors
SET id = 3
WHERE id = 1;
```

как и после выполнения приведенного ниже запроса:

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
        ON UPDATE SET NULL
        ON DELETE SET NULL
);

INSERT INTO Authors (name, surname)
VALUES ('Stephen', 'King'),
       ('Joseph', 'Conrad');
       
INSERT INTO Books (title, author_id)
VALUES ('It', 1),
       ('Heart of Darkness', 2),
       ('Pet Sematary', 1);

DELETE FROM Authors
WHERE id = 1;
```

таблица `Books` будет иметь вид:

```no-highlight
+----+-------------------+-----------+
| id | title             | author_id |
+----+-------------------+-----------+
| 1  | It                | NULL      |
| 2  | Heart of Darkness | 2         |
| 3  | Pet Sematary      | NULL      |
+----+-------------------+-----------+
```

В этих двух запросах, в отличие от двух примеров выше, используется ключевое слово `SET NULL`, поэтому изменение и удаление записи таблицы `Authors` приводит к тому, что связанные с ней записи таблицы `Books` в качестве значения внешнего ключа просто принимают значение `NULL`.

Операторы `ON UPDATE` и `ON DELETE` можно использовать как вместе, так и отдельно. Также каждый из операторов может задавать собственное поведение, например, `ON UPDATE` — `CASCADE`, а `ON DELETE` — `SET NULL`.
Внешний ключ может состоять как из одного, так и из нескольких полей. Разница лишь в том, что в первом случае в скобках после ограничения `FOREIGN KEY` и названия родительской таблицы указывается только одно поле, а во втором случае перечисляются несколько полей через запятую.

Таким образом, синтаксис ограничения `FOREIGN KEY` в общем виде имеет следующий вид:

```no-highlight
FOREIGN KEY (<1 поле>, <2 поле>, ...) REFERENCES <имя родительской таблицы> (<1 поле>, <2 поле>, ...)
```
#sql 
