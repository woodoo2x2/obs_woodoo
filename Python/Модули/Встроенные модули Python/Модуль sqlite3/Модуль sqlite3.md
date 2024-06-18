Шаблон создания базы данных

```py
import sqlite3 as sq
 
with sq.connect("cars.db") as con:
    cur = con.cursor()
 
    cur.execute("""CREATE TABLE IF NOT EXISTS cars (
        car_id INTEGER PRIMARY KEY AUTOINCREMENT,
        model TEXT,
        price INTEGER
    )""")
```

Когда контекстный менеджер завершает свою работу, он автоматически выполняет два метода:

- `con.commit()` – применение всех изменений в таблицах БД;
- `con.close() `– закрытие соединения с БД.

Это необходимые действия для сохранения внесенных изменений в БД

### Методы execute, executemany и executescript

Давайте добавим в таблицу cars несколько записей. В самом простом случае это можно сделать так:

```py
cur.execute("INSERT INTO cars VALUES(1,'Audi',52642)")
cur.execute("INSERT INTO cars VALUES(2,'Mercedes',57127)")
cur.execute("INSERT INTO cars VALUES(3,'Skoda',9000)")
cur.execute("INSERT INTO cars VALUES(4,'Volvo',29000)")
cur.execute("INSERT INTO cars VALUES(5,'Bentley',350000)")
```

В результате, таблица будет содержать данные:

![](https://proproprogs.ru/htm/modules/files/metody-execute-executemany-executescript-commit-rollback.files/image001.png)


Однако, когда мы программируем на Python, то данные, как правило, хранятся в каких-либо коллекциях, например, так:

```py
cars = [
    ('Audi', 52642),
    ('Mercedes', 57127),
    ('Skoda', 9000),
    ('Volvo', 29000),
    ('Bentley', 350000)
]
```

И мы бы хотели брать значения из этого списка и передавать их в [[SQL]]-запрос. Для этого запрос следует записывать в виде следующего шаблона:

```py
cur.execute("INSERT INTO cars VALUES(NULL, ?, ?)", cars[0])
```

Здесь вместо знаков вопроса будут подставлены соответствующие данные из первого кортежа списка. Соответственно, весь набор ранее приведенных строчек, можно заменить циклом:

```py
for car in cars:
    cur.execute("INSERT INTO cars VALUES(NULL, ?, ?)", car)
```

Или, еще проще, воспользоваться методом executemany, который специально для этого и существует:

```py
cur.executemany("INSERT INTO cars VALUES(NULL, ?, ?)", cars)
```

Сразу же здесь отмечу, что помимо знаков вопроса можно использовать именованные параметры (плейсхолдеры). Для этого в запросе перед ними ставится двоеточие, а затем, указывается словарь, где имя – это ключ, вместо которого будет подставлено его значение:

```py
cur.execute("UPDATE cars SET price = :Price WHERE model LIKE 'A%'", {'Price': 0})
```
Далее, если нужно выполнить несколько отдельных SQL-команд, то можно передать их СУБД с помощью метода executescript:
```py
cur.executescript("""DELETE FROM cars WHERE model LIKE 'A%';
    UPDATE cars SET price = price+1000
""")
```

Мы здесь сначала удаляем все записи, у которых модель начинается на букву A, а затем у оставшихся записей увеличиваем цену на 1000. Причем, команды должны отделяться друг от друга точкой с запятой.

У этого метода есть одно ограничение: здесь нельзя использовать шаблоны запросов, как мы это делали в предыдущих методах. В `executescript` буквально записываются SQL-запросы как есть со всеми данными.

## Методы commit и rollback

Давайте теперь реализуем соединение с БД через блок обработки исключений `try/except/finally:`

```py
con = None
try:
    con = sq.connect("cars.db")
    cur = con.cursor()
 
    cur.executescript("""CREATE TABLE IF NOT EXISTS cars (
            car_id INTEGER PRIMARY KEY AUTOINCREMENT,
            model TEXT,
            price INTEGER
        );
        BEGIN;
        INSERT INTO cars VALUES(NULL,'Audi',52642);
        INSERT INTO cars VALUES(NULL,'Mercedes',57127);
        INSERT INTO cars VALUES(NULL,'Skoda',9000);
        INSERT INTO cars VALUES(NULL,'Volvo',29000);
        INSERT INTO cars VALUES(NULL,'Bentley',350000);
        UPDATE cars SET price = price+1000
    """)
    con.commit()
 
except sq.Error as e:
    if con: con.rollback()
    print("Ошибка выполнения запроса")
finally:
    if con: con.close()
```

В чем преимущество такого подхода? Смотрите, мы здесь сами «вручную» вызываем методы commit и close. Если операции с таблицами прошли успешно, то они будут сохранены, если же возникли какие-либо ошибки (исключения), то будет вызван метод rollback, который откатывает состояние БД в состояние отметки BEGIN, то есть, все внесенные изменения применены не будут.

Например, укажем в команде UDPATE неверное имя таблицы:

```py
UPDATE cars2 SET price = price+1000
```
При запуске программы произойдет ошибка выполнения запроса и состояние БД не изменится, то есть, все новые добавленные записи не появятся в таблице cars. А вот если вместо rollback указать commit, то увидим добавление записей. То есть, при использовании менеджера контекста в данном случае не выполнилась бы только последняя команда, но отката состояния БД не произошло бы. Вот так более тонко можно управлять состоянием таблиц в БД.

Если при работе с БД предполагается сохранять вносимые изменения сразу после выполнения SQL-запроса, то это можно сделать с помощью метода connect, установив в нем параметр isolation_level=None:

```py
with sq.connect("cars.db", isolation_level=None) as con:
    cur = con.cursor()
 
    cur.executescript("""INSERT INTO cars VALUES(NULL,'Audi',52642);
        INSERT INTO cars VALUES(NULL,'Mercedes',57127);
        INSERT INTO cars VALUES(NULL,'Skoda',9000);
        INSERT INTO cars VALUES(NULL,'Volvo',29000);
        INSERT INTO cars VALUES(NULL,'Bentley',350000);
    """)
```
Однако так делать без особой надобности не стоит, т.к. это уменьшает скорость работы с БД из-за постоянной записи данных непосредственно в файл. Без изменения этого параметра все изменения сохраняются в памяти, а потому работа происходит куда быстрее.

## Свойство lastrowid

Давайте теперь представим, что у нас есть еще одна таблица cust, которая содержит покупателей машин. Причем, если происходит покупка по «trade-in», то прежняя машина владельца добавляется в конец таблицы cars, а в таблице cust появляется запись с именем покупателя, идентификатором машины сданной в «trade-in» и id новой купленной машины:

![](https://proproprogs.ru/htm/modules/files/metody-execute-executemany-executescript-commit-rollback.files/image002.jpg)

Чтобы реализовать SQL-запрос добавления записи в таблицу cust, нам нужно знать car_id автомобиля сданного в «trade-in». Предположим, что Федор еще не совершил покупку и таблица cars не содержит запись с его сданным автомобилем. Добавим ее. Выполним следующий запрос вот в такой программе:

```py
with sq.connect("cars.db") as con:
    cur = con.cursor()
 
    cur.executescript("""CREATE TABLE IF NOT EXISTS cars (
            car_id INTEGER PRIMARY KEY AUTOINCREMENT,
            model TEXT,
            price INTEGER
        );
        CREATE TABLE IF NOT EXISTS cust(name TEXT, tr_in INTEGER, buy INTEGER);        
    """)
 
    cur.execute("INSERT INTO cars VALUES(NULL,'Запорожец', 1000)")
```
Мы здесь создаем еще одну таблицу cust с тремя полями и, затем, добавляем в таблицу cars автомобиль «Запорожец», который сдает покупатель Федор. Как теперь нам узнать car_id этой записи? Для этого можно воспользоваться специальным свойством:

```py
last_row_id = cur.lastrowid
```

которое содержит значение rowid последней добавленной записи. В нашем случае поля car_id и rowid будут совпадать, поэтому воспользуемся этим значением и сформируем еще один запрос на добавление записи во вторую таблицу:
```py
buy_car_id = 2
 
cur.execute("INSERT INTO cust VALUES('Федор', ?, ?)", (last_row_id, buy_car_id))
```
Теперь, при выполнении нашей программы в таблице cust увидим искомую запись. Вот так используется свойство lastrowid.


# Функции fetchall(), fetchmany(size), fetchone()
Продолжим изучение API для работы с SQLite на языке Python и пару слов о способе извлечения данных из запросов. Об этом мы уже говорили на одном из предыдущих занятий, когда рассматривали методы:

- `fetchall()` – возвращает число записей в виде упорядоченного списка;
- `fetchmany(size)` – возвращает число записей не более size;
- `fetchone()` – возвращает первую запись.



```py
import sqlite3 as sq
 
cars = [
    ('Audi', 52642),
    ('Mercedes', 57127),
    ('Skoda', 9000),
    ('Volvo', 29000),
    ('Bentley', 350000)
]
 
with sq.connect("cars.db") as con:
    cur = con.cursor()
 
    cur.executescript("""CREATE TABLE IF NOT EXISTS cars (
            car_id INTEGER PRIMARY KEY AUTOINCREMENT,
            model TEXT,
            price INTEGER)
    """)
 
    cur.executemany("INSERT INTO cars VALUES(NULL,?, ?)", cars)
```

И, затем, выполним запрос на выборку записей:

```py
cur.execute("SELECT model, price FROM cars")
```

Чтобы в программе на Python получить доступ к сформированной выборке, как раз и нужно воспользоваться функциями:

`fetchall`, `fetchmany` или `fetchone`

следующим образом:

```py
rows = cur.fetchall()
print(rows)
```
В консоли увидим список из кортежей с данными записей в соответствии с указанными полями model и price:

```py
[('Audi', 52642), ('Mercedes', 57127), ('Skoda', 9000), ('Volvo', 29000), ('Bentley', 350000)]
```

По аналогии для fetchone:

```py
rows = cur.fetchone()
```

будет взята только первая запись. И для fetchmany:

```py
rows = cur.fetchmany(4)
```

берем не более четырех первых записей:

```py
[('Audi', 52642), ('Mercedes', 57127), ('Skoda', 9000), ('Volvo', 29000)]
```

Наконец, мы говорили, что после формирования выборки сам экземпляр класса Cursor можно использовать как итерируемый объект и выбирать записи в цикле:

```py
for result in cur:
    print(result)
```

Преимущество такого подхода в экономии памяти при большом числе записей в выборке. Здесь на каждой итерации цикла мы выбираем только одну следующую запись, а не храним их сразу целиком в памяти в виде списка. Это часто бывает эффективно и очень удобно.

Далее, иногда более предпочтительным вариантом выходных данных является не кортеж с данными, а словарь, позволяющий обращаться к элементам по именам полей. Для этого после установления соединения с БД следует прописать вот такую строчку:
```py
con.row_factory = sq.Row
```
Теперь, при выполнении программы увидим, что переменная result в цикле ссылается на объект Row, а не кортеж:

```py
<sqlite3.Row object at 0x00000185B226B8B0>
```

И через этот объект доступ к данным осуществляется с помощью имен полей таблицы cars:
```py
print(result['model'], result['price'])
```

## Хранение изображений в БД

Часто в БД требуется хранить небольшие изображения, например, аватары пользователей. Для этого имеется специальный тип данных `BLOB`. Давайте создадим таблицу users с полем ava:

```py
cur.executescript("""CREATE TABLE IF NOT EXISTS users (
    name TEXT,
    ava BLOB,
    score INTEGER)
""")
```

Далее напишем вспомогательную функцию считывания изображения из файла:

```py
def readAva(n):
    try:
        with open(f"avas/{n}.png", "rb") as f:
            return f.read()
    except IOError as e:
        print(e)
        return False
```

Если изображение было успешно прочитано, то функция возвратит набор двоичных данных, иначе значение False. Затем, в менеджере контекста БД вызовем ее и при успешном чтении данных записываем в таблицу users:

```py
img = readAva(1)
if img:
    binary = sq.Binary(img)
    cur.execute("INSERT INTO users VALUES ('Николай', ?, 1000)", (binary,))
```

Обратите внимание, прежде чем бинарные данные передавать в поле BLOB их нужно закодировать в бинарный объект модуля SQLite. Для этого и вызывается метод Binary, которому передается последовательность прочитанных байт изображения.

Запустим программу и перейдем в приложение `DB Browser`. Откроем там таблицу users. И при выборе поля `BLOB` справа увидим изображение, которое там хранится:

![](https://proproprogs.ru/htm/modules/files/metody-fetchall-fetchmany-fetchone-iterdump.files/image001.jpg)

Давайте теперь прочитаем изображение из этого поля. Выполним следующий запрос:

```py
cur.execute("SELECT ava FROM users LIMIT 1")
```

и с помощью метода fetchone обратимся к первой (и единственной) записи и возьмем данные из поля ava:

```py
img = cur.fetchone()['ava']
```

Чтобы нам знать, что изображение было успешно прочитано, сохраним его в файл. Для этого запишем вот такую функцию:

```py
def writeAva(name, data):
    try:
        with open(name, "wb") as f:
            f.write(data)
    except IOError as e:
        print(e)
        return False
 
    return True
```

И вызовем ее:

```py
writeAva("out.png", img)
```

У нас в рабочем каталоге программы появился файл out.png и при его просмотре видим, что это то самое изображение. Вот так производится запись и чтение бинарных данных в SQLite.

## Создание бэкапа БД

Класс Cursor содержит один весьма полезный метод

```py
iterdump()
```

возвращающий итератор для SQL-запросов, на основе которых можно воссоздать текущую БД. Если просто вывести в консоль возвращаемых строк:

```py
with sq.connect("cars.db") as con:
    cur = con.cursor()
 
    for sql in con.iterdump():
        print(sql)
```

То получим список следующих SQL-команд:

```sql
BEGIN TRANSACTION;
CREATE TABLE cars (
            car_id INTEGER PRIMARY KEY AUTOINCREMENT,
            model TEXT,
            price INTEGER);
INSERT INTO "cars" VALUES(1,'Audi',0);
INSERT INTO "cars" VALUES(2,'Mercedes',57127);
INSERT INTO "cars" VALUES(3,'Skoda',9000);
INSERT INTO "cars" VALUES(4,'Volvo',29000);
INSERT INTO "cars" VALUES(5,'Bentley',350000);
DELETE FROM "sqlite_sequence";
INSERT INTO "sqlite_sequence" VALUES('cars',5);
CREATE TABLE users (
            name TEXT,
            ava BLOB,
            score INTEGER);
INSERT INTO "users" VALUES('Николай', …,1000);
COMMIT;
```

С помощью этих запросов можно точно воссоздать таблицы БД, то есть, их можно рассматривать как некий дамп БД.

Чтобы наша программа выглядела более функциональной, сохраним все эти строчки в отдельном файле:

```py
    with open("sql_damp.sql", "w") as f:
        for sql in con.iterdump():
            f.write(sql)
```

После запуска в рабочем каталоге программы появится файл sql_damp.sql с набором соответствующих команд.

Теперь, чтобы восстановить БД с помощью этого файла можно воспользоваться методом executescript, о котором мы уже говорили:

```pt
    with open("sql_damp.sql", "r") as f:
        sql = f.read()
        cur.executescript(sql)
```

Перед выполнением этой программы удалим файл cars.db и после запуска снова увидим этот файл с прежним содержимым.


## Создание БД в памяти

Интересной особенностью модуля SQLite является возможность создания БД непосредственно в памяти. Такая организация позволяет хранить временные данные программы в формате таблиц и работать с ними через SQL-запросы. В ряде случаев это бывает весьма удобно.

Для создания БД в памяти устройства подключение записывается в виде:

```py
data = [("car", "машина"), ("house", "дом"), ("tree", "дерево"), ("color", "цвет")]
 
con = sq.connect(':memory:')
with con:
    cur = con.cursor()
    cur.execute("""CREATE TABLE IF NOT EXISTS dict(
        eng TEXT, rus TEXT    
    )""")
 
    cur.executemany("INSERT INTO dict VALUES(?,?)", data)
 
    cur.execute("SELECT rus FROM dict WHERE eng LIKE 'c%'")
    print(cur.fetchall())
```
Мы здесь создали подключение, указав специальный параметр «:memory:», что означает «память» и, затем, в менеджере контекста создали таблицу dict с двумя полями, заполнили ее значениями и сделали выборку всех английских слов, начинающихся с первой буквы ‘c’.

Этот пример показывает как можно использовать богатые возможности СУБД для хранения и выборки данных в процессе работы приложения, не создавая на диске БД.

На этом мы завершим цикл занятий по SQLite. Данного материала вам вполне хватит для большинства приложений. Ну а по мере его использования неминуемо узнаете многие другие нюансы работы данного модуля.