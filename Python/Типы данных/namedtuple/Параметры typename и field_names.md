
Параметр `typename` отвечает за имя создаваемого типа, параметр `field_names` за названия полей. Имя типа — это строка с типом, который нужно сделать именованным кортежем. В качестве параметра `field_names` можно использовать:

- список
- словарь
- кортеж
- строка
- множество

**Параметр `field_names` является списком:**

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])    # в качестве второго параметра передаем список
point =  Point(2, 4)
print(point)                               # выводит Point(x=2, y=4)
```

**Параметр `field_names` является кортежем:**

```python
from collections import namedtuple

Point = namedtuple('Point', ('x', 'y'))    # в качестве второго параметра передаем кортеж
point =  Point(2, 4)
print(point)                               # выводит Point(x=2, y=4)
```

**Параметр `field_names` является словарем:**

В этом случае для полей именованного кортежа используются **ключи словаря**, поэтому в качестве значений можно указать, все что угодно.

```python
from collections import namedtuple

Point = namedtuple('Point', {'x': 0, 'y': 69})    # в качестве второго параметра передаем словарь
point =  Point(2, 4)
print(point)                                      # выводит Point(x=2, y=4)
```

**Параметр `field_names` является строкой:**

При создании именованного кортежа с помощью строки мы указываем поля либо через символ пробела, либо разделяя их символом `,`.

```python
from collections import namedtuple

Point = namedtuple('Point', 'x y')    # в качестве второго параметра передаем строку
point =  Point(2, 4)
print(point)                          # выводит Point(x=2, y=4)
```

либо:

```python
from collections import namedtuple

Point = namedtuple('Point', 'x,y')     # в качестве второго параметра передаем строку
point =  Point(2, 4)
print(point)                           # выводит Point(x=2, y=4)
```

**Параметр `field_names` является множеством:**

Мы можем создать именованный кортеж с помощью множества, однако делать это не рекомендуется. Как мы знаем, множество – это неупорядоченный набор данных. Когда мы используем неупорядоченный набор данных для предоставления полей именованному кортежу, мы можем получить неожиданный результат.

Результатом работы кода:

```python
from collections import namedtuple

Point = namedtuple('Point', {'x', 'y'})    # в качестве второго параметра передаем множество
point =  Point(2, 4)
print(point)
```

может быть как:

```no-highlight
Point(x=2, y=4)
```

так и:

```no-highlight
Point(y=2, x=4)
```

В приведенном выше примере имена координат меняются местами, что может не подходить для вашего варианта использования.

В качестве параметра `field_names` можно передавать любой итерируемый объект, например, результат вызова функций `map()` и `filter()`.

Обратите также внимание на то, что создавать именованные кортежи можно не только с помощью позиционных аргументов, но и с помощью именованных.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
point1 = Point(2, 4)                          # позиционные аргументы
point2 = Point(y=10, x=3)                     # именованные аргументы

print(point1)
print(point2)
```

выводит:

```no-highlight
Point(x=2, y=4)
Point(x=3, y=10)
```

В качестве названия полей для именованных кортежей мы можем использовать любое корректное название имени переменной, за исключением:

- имен, начинающихся с подчеркивания (`_`)
- ключевых слов языка Python (`if, with, else, class, ...`)

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', '_y'])
```

приводит к возникновению ошибки: `ValueError: Field names cannot start with an underscore: '_y'`.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'if'])
```

приводит к возникновению ошибки: `ValueError: Type names and field names cannot be a keyword: 'if'`.
