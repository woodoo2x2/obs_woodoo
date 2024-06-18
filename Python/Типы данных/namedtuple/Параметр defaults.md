
Параметр `defaults` используется для того, чтобы установить значения по умолчанию для полей именованного кортежа.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], defaults=(0, 0))
point1 = Point()      # используем значения по умолчанию
point2 = Point(1, 9)

print(point1)
print(point2)
```

выводит:

```no-highlight
Point(x=0, y=0)
Point(x=1, y=9)
```

Можно указать значение по умолчанию только для некоторых полей, при этом `defaults` присваивает значения по умолчанию с конца.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], defaults=(0,))
point =  Point(7)      # используем значения по умолчанию для y
print(point)
```

выводит:

```no-highlight
Point(x=7, y=0)
```

 Имейте в виду, что параметр `defaults` работает только в Python 3.7+.
