

В предыдущих курсах мы нередко сталкивались с типами данных, объекты которых имеют различные атрибуты и методы. Например, объекты типа `date` из модуля `datetime` обладают немалым количеством полезных атрибутов и методов.

 Тип данных и класс — это одно и то же.

Приведенный ниже код:

```python
from datetime import date

today = date(2022, 9, 25)

print(today.year)                 # год
print(today.month)                # месяц
print(today.day)                  # день
print(today.isoweekday())         # порядковый номер дня недели
print(today.toordinal())          # номер дня, соответствующий дате 2022-09-25
```

выводит:

```no-highlight
2022
9
25
7
738423
```

В примере выше `year, month` и `day` являются атрибутами, а `isoweekday()` и `toordinal()` — методами.

Для получения списка всех атрибутов и методов объекта можно воспользоваться встроенной функцией [[Функция dir()|dir()]].

Приведенный ниже код:

```python
from datetime import date

today = date(2022, 9, 25)

print(dir(today))
```

выводит:

```no-highlight
['__add__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__radd__', '__reduce__', '__reduce_ex__', '__repr__', '__rsub__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', 'ctime', 'day', 'fromisocalendar', 'fromisoformat', 'fromordinal', 'fromtimestamp', 'isocalendar', 'isoformat', 'isoweekday', 'max', 'min', 'month', 'replace', 'resolution', 'strftime', 'timetuple', 'today', 'toordinal', 'weekday', 'year']
```