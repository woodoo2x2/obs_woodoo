

Создадим класс `UpperCaseDict`, наследника класса [[Тип данных dict]], который автоматически сохраняет все ключи в виде строк, в которых все буквы будут в верхнем регистре.

Для реализации класса `UpperCaseDict` мы переопределим магический метод `__setitem__()` и для удобства `__repr__()`:

```python
class UpperCaseDict(dict):
    def __setitem__(self, key, value):
        key = str(key).upper()
        super().__setitem__(key, value)
    
    def __repr__(self):
        return f'{type(self).__name__}({super().__repr__()})'
```

Установка элементов в этом словаре работает так, как мы и ожидаем.

Приведенный ниже код:

```python
numbers = UpperCaseDict()
numbers['one'] = 1
numbers['two'] = 2

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'ONE': 1, 'TWO': 2})
```

Однако если мы вызовем метод `update()`, то получим не совсем ожидаемое поведение.

Приведенный ниже код:

```python
numbers = UpperCaseDict()
numbers['one'] = 1
numbers['two'] = 2

numbers.update({'three': 3})

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'ONE': 1, 'TWO': 2, 'three': 3})
```

Как мы видим, при вызове метода `update()` магический метод `__setitem__()`, который мы переопределили, не вызывается, и в словарь `UpperCaseDict` добавляется пара `'three': 3`, имеющая ключ в нижнем регистре.

Мы можем исправить это поведение с помощью переопределения метода `update()`:

```python
class UpperCaseDict(dict):
    def update(self, items):
        if isinstance(items, dict):
            items = items.items()
        for key, value in items:
            self[key] = value

    def __setitem__(self, key, value):
        key = str(key).upper()
        super().__setitem__(key, value)
    
    def __repr__(self):
        return f'{type(self).__name__}({super().__repr__()})'
```

Теперь приведенный ниже код:

```python
numbers = UpperCaseDict()
numbers['one'] = 1
numbers['two'] = 2

numbers.update({'three': 3})

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'ONE': 1, 'TWO': 2, 'THREE': 3})
```

Не совсем ожидаемое поведение мы получим и при создании словаря с некоторыми начальными значениями.

Приведенный ниже код:

```python
numbers = UpperCaseDict(one=1, two=2, three=3)

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'one': 1, 'two': 2, 'three': 3})
```

Как мы видим, в словарь `UpperCaseDict` добавляются три пары `'one': 1, 'two': 2` и `'three': 3`, имеющие ключи в нижнем регистре.

Мы можем исправить это поведение с помощью переопределения метода  `__init__()`:

```python
class UpperCaseDict(dict):
    def __init__(self, items=(), **kwargs):
        self.update(items)
        self.update(kwargs)

    def update(self, items):
        if isinstance(items, dict):
            items = items.items()
        for key, value in items:
            self[key] = value

    def __setitem__(self, key, value):
        key = str(key).upper()
        super().__setitem__(key, value)
    
    def __repr__(self):
        return f'{type(self).__name__}({super().__repr__()})'
```

Теперь приведенный ниже код:

```python
numbers = UpperCaseDict(one=1, two=2, three=3)

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'ONE': 1, 'TWO': 2, 'THREE': 3})
```

Не совсем ожидаемое поведение мы получим и при использовании метода `setdefault()`, поскольку данный метод также не вызывает магический метод `__setitem__()`.

Приведенный ниже код:

```python
numbers = UpperCaseDict()

numbers.setdefault('one', 1)

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'one': 1})
```

Мы можем исправить это поведение с помощью переопределения метода `setdefault()`:

```python
class UpperCaseDict(dict):
    def __init__(self, items=(), **kwargs):
        self.update(items)
        self.update(kwargs)

    def update(self, items):
        if isinstance(items, dict):
            items = items.items()
        for key, value in items:
            self[key] = value

    def setdefault(self, key, value):
        if str(key).upper() not in self:
            self[key] = value

    def __setitem__(self, key, value):
        key = str(key).upper()
        super().__setitem__(key, value)
    
    def __repr__(self):
        return f'{type(self).__name__}({super().__repr__()})'
```

Как мы видим, для правильной работы класса `UpperCaseDict` необходимо переопределить много методов базового типа `dict`. При наследовании от `dict` мы ожидаем, что методы `update(), __init__(), setdefault()` вызовут метод `__setitem__()`, однако этого не происходит.