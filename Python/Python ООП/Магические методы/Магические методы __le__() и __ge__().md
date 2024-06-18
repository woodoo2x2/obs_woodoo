

Помимо строгих сравнений на больше/меньше существуют нестрогие сравнения, которые допускают равенство (операторы `>=, <=`). Данные операции реализуются с помощью магических методов `__le__()` и `__ge__()`.

Магический метод `__le__()` отвечает за сравнение на меньше или равно. То есть выражение `fruit1 <= fruit2` равносильно вызову `fruit1.__le__(fruit2)`. За сравнение на больше или равно отвечает магический метод `__ge__()`, и аналогично предыдущему методу выражение `fruit1 >= fruit2` равносильно вызову `fruit1.__ge__(fruit2)`.

Приведенный ниже код:

```python
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass

    def __eq__(self, other):
        if isinstance(other, Fruit):
            return self.mass == other.mass
        return NotImplemented

    def __le__(self, other):
        if isinstance(other, Fruit):
            return self.mass <= other.mass
        return NotImplemented

    def __ge__(self, other):
        if isinstance(other, Fruit):
            return self.mass >= other.mass
        return NotImplemented


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)
fruit3 = Fruit('груша', 150)

print(fruit1 <= fruit2)
print(fruit1 >= fruit2)
print(fruit1 <= fruit3)
print(fruit1 >= fruit3)
```

выводит:

```no-highlight
True
False
True
True
```

Если в классе реализовано сравнение на больше (меньше), то сравнение на меньше (больше) для объектов этого класса можно считать реализованным автоматически. Аналогично себя ведут и нестрогие сравнения на больше/меньше.

Приведенный ниже код:

```python
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass

    def __lt__(self, other):
        if isinstance(other, Fruit):
            return self.mass < other.mass
        return NotImplemented


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)

print(fruit1 < fruit2)
print(fruit1 > fruit2)
```

выводит:

```no-highlight
True
False
```

Дело в том, что выражение `fruit1 > fruit2` приводит к вызову метода `__gt__()` у `fruit1`, который возвращает константу `NotImplemented`, так как метод не реализован. Далее Python пытается получить значение выражения `fruit2 < fruit1`, что приводит к вызову метода `__lt__()` у `fruit2`, и так как данный метод уже реализован, возвращается результат сравнения в виде значения `False`.

Несмотря на данную особенность реализации, рекомендуется явно определять все методы сравнения вручную либо с помощью декоратора [[Декоратор @total_ordering]].

После реализации в классе операторов сравнения на больше/меньше появляется возможность сортировать экземпляры этого класса и выбирать среди них наименьший/наибольший.

Приведенный ниже код:

```python
from functools import total_ordering


@total_ordering
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass

    def __repr__(self):
        return f'Fruit({repr(self.name)}, {self.mass})'

    def __eq__(self, other):
        if isinstance(other, Fruit):
            return self.mass == other.mass
        return NotImplemented

    def __lt__(self, other):
        if isinstance(other, Fruit):
            return self.mass < other.mass
        return NotImplemented


fruits = [Fruit('яблоко', 180), Fruit('груша', 160), Fruit('авокадо', 170), Fruit('банан', 150)]

print(sorted(fruits))
print(min(fruits))
print(max(fruits))
```

выводит:

```python
[Fruit('банан', 150), Fruit('груша', 160), Fruit('авокадо', 170), Fruit('яблоко', 180)]
Fruit('банан', 150)
Fruit('яблоко', 180)
```

Любая операция сравнения реализуется с помощью соответствующего магического метода. Методы для сравнения на равенство есть у любого класса по умолчанию, в то время как методы для сравнения на больше/меньше необходимо реализовывать самостоятельно, и если этого не сделать, при попытке сравнения двух объектов на больше/меньше будет возбуждено исключение.

Приведенный ниже код:

```python
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)

print(fruit1 < fruit2)
```

приводит к возбуждению исключения:

```no-highlight
TypeError: '<' not supported between instances of 'Fruit' and 'Fruit'
```
 Магические методы `__eq__(), __ne__(), __gt__(), __lt__(), __ge__()` и `__le__()` буквально соответствуют английским equal (равно), not equal (не равно), greater than (больше, чем), less than (меньше, чем), greater than or equal (больше или равно) и less than or equal (меньше или равно) соответственно.