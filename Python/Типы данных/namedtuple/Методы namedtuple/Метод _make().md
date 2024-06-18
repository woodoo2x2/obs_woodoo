

Метод `_make()` используется для создания именованных кортежей из итерируемых объектов (список, кортеж, строка, словарь и т.д.).

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person._make(['Timur', 29, 170])

print(timur)
```

выводит:

```no-highlight
Person(name='Timur', age=29, height=170)
```

Обратите внимание на то, что метод `_make()` – это метод типа, а не конкретного экземпляра, поэтому вызывать его нужно через название типа (`Person._make`). Метод `_make()` работает как альтернативный конструктор типа.
