

Рассмотрим класс `Point`, описывающий точку на плоскости, и создадим два экземпляра этого класса:

```python
class Point:
    def __init__(self, x, y):
        self.x = x                       # координата точки по оси x
        self.y = y                       # координата точки по оси y


p1 = Point(1, 2)
p2 = Point(1, 2)
```

Точки `p1` и `p2` являются разными объектами, и мы можем в этом убедиться, сравнив их с помощью оператора `is`.

Приведенный ниже код:

```python
print(p1 is p2)
```

выводит:

```no-highlight
False
```

Если мы попытаемся сравнить два данных объекта с помощью оператора `==`, мы также получим в качестве результата `False`, несмотря на то что оба объекта имеют равные значения атрибутов.

Приведенный ниже код:

```python
print(p1 == p2)
```

выводит:

```no-highlight
False
```

Дело в том, что сравнение на равенство по умолчанию является сравнением на идентичность. Другими словами, если в классе не определено, как будет происходить сравнение с помощью оператора `==`, оно будет равносильно сравнению с помощью `is`.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y


p1 = Point(1, 2)
p2 = Point(1, 2)

print(p1 == p1)
print(p2 == p2)
```

выводит:

```no-highlight
True
True
```

Чтобы определить, как будет происходить сравнение на равенство, нам требуется определить в классе метод `__eq__()`. Когда мы сравниваем два объекта с помощью оператора `==`, Python автоматически вызывает данный метод у левого операнда, передавая методу в качестве аргумента правый операнд. То есть выражение `p1 == p2` равносильно вызову `p1.__eq__(p2)`.

Для возможности сравнивать экземпляры класса `Point` определим в нем метод `__eq__()`. Будем считать точки равными, если они имеют равные координаты по обеим осям.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y


p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 == p2)
print(p1 == p3)
print(p2 == p3)
```

выводит:

```no-highlight
True
False
False
```

Как мы видим, теперь равенство объектов определяется иначе, а именно на основании их координат. Точка `p1` равна точке `p2`, так как их координаты совпадают, в то время как точки `p1` и `p2` не равны точке `p3`, так как их координаты не совпадают.

Однако определенный нами метод `__eq__()` работает верно лишь в том случае, когда мы сравниваем объект типа `Point` с объектом такого же типа. Если мы попытаемся сравнить точку `p1`, скажем, с кортежем, мы получим ошибку.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y


p1 = Point(1, 2)

print(p1 == (1, 2))
```

приводит к возбуждению исключения:

```no-highlight
AttributeError: 'tuple' object has no attribute 'x'
```

Чтобы это исправить, мы можем добавить в метод `__eq__()` проверку на то, что объект, с которым происходит сравнение, принадлежит необходимому типу. В противном случае метод может возбуждать исключение или возвращать значение `False`.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False


p1 = Point(1, 2)

print(p1 == (1, 2))
```

выводит:

```no-highlight
False
```

Аналогично сравнению на равенство для сравнения на неравенство с помощью оператора `!=` используется магический метод `__ne__()`.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False

    def __ne__(self, other):
        if isinstance(other, Point):
            return self.x != other.x or self.y != other.y
        return True


p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 != p2)
print(p1 != p3)
print(p2 != p3)
```

выводит:

```no-highlight
False
True
True
```

Важно отметить, что Python автоматически реализует метод `__ne__()`, если метод `__eq__()` уже реализован.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False


p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 != p2)
print(p1 != p3)
print(p2 != p3)
```

выводит:

```no-highlight
False
True
True
```

Однако если нам требуется несколько иная реализация, мы всегда можем определить метод `__ne__()` вручную.

При реализованном методе `__ne__()` метод `__eq__()` автоматически не реализуется.


 Метод `__eq__()`, помимо сравнения с помощью оператора `==`, также вызывается при проверке на принадлежность с помощью оператора `in`.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        print('Вызов метода __eq__()')
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False


p = Point(2, 2)        
points = [Point(1, 1), Point(2, 2), Point(3, 3)]

print(p in points)
```

выводит:

```no-highlight
Вызов метода __eq__()
Вызов метода __eq__()
True
```


 Обычно при переопределении магического метода `__eq__()` также переопределяют метод [[Магический метод __hash__()]].

## Сравнение обектов
[[Сравнение объектов]]