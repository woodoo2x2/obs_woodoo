## Типы UserDict и UserList

Для решения указанных выше проблем, связанных с наследованием от типов `dict` и `list`, можно использовать типы `UserDict` и `UserList` из модуля [[Модуль collenctions]].

### Наследование от UserDict

Класс `UserDict` – это удобная обертка для обычного объекта `dict`. Этот класс обеспечивает то же поведение, что и [[Тип данных dict]], с дополнительной возможностью предоставления доступа к базовому словарю через атрибут экземпляра `data`.

Перепишем наш класс `UpperCaseDict`, который автоматически сохраняет все ключи в виде строк, в которых все буквы будут в верхнем регистре, так, чтобы он наследовался от `UserDict`:

```python
from collections import UserDict

class UpperCaseDict(UserDict):
    def __setitem__(self, key, value):
        key = str(key).upper()
        self.data.__setitem__(key, value)

    def __repr__(self):
        return f'{type(self).__name__}({self.data.__repr__()})'
```

Приведенный ниже код:

```python
numbers = UpperCaseDict({'one': 1, 'two': 2})

numbers['three'] = 3
numbers.update({'four': 4})
numbers.setdefault('five', 5)

print(numbers)
```

выводит:

```no-highlight
UpperCaseDict({'ONE': 1, 'TWO': 2, 'THREE': 3, 'FOUR': 4, 'FIVE': 5})
```

Как мы видим, теперь все методы класса `UpperCaseDict` работают корректно. Нет необходимости предоставлять собственные реализации методов `__init__(), update()` и `setdefault()`. Это связано с тем, что в `UserDict` все методы, обновляющие существующие ключи или добавляющие новые, полагаются на пользовательскую версию метода `__setitem__()`.

Наиболее заметным отличием между `UserDict` и `dict` является атрибут `data`, который содержит обернутый словарь. Использование атрибута `data` напрямую может сделать код более простым, так как не нужно постоянно вызывать функцию `super()` для обеспечения нужной функциональности базового класса.

### Наследование от UserList

Класс `UserList` – это удобная обертка для обычного объекта [[Тип данных list]]. Этот класс обеспечивает то же поведение, что `list`, с дополнительной возможностью предоставления доступа к базовому списку через атрибут экземпляра `data`.

Наследники класса `UserList` должны предоставить инициализатор, который можно вызывать либо без аргументов, либо с одним аргументом, определяющим начальный набор элементов.

Перепишем наш класс `StringList`, который автоматически сохраняет все свои элементы в виде строк, так, чтобы он наследовался от `UserList`.

```python
from collections import UserList

class StringList(UserList):
    def __init__(self, iterable):
        super().__init__(str(item) for item in iterable)

    def __setitem__(self, index, item):
        self.data[index] = str(item)

    def insert(self, index, item):
        self.data.insert(index, str(item))

    def append(self, item):
        self.data.append(str(item))

    def extend(self, other):
        if isinstance(other, type(self)):
            self.data.extend(other)
        else:
            self.data.extend(str(item) for item in other)

    def __repr__(self):
        return f'{type(self).__name__}({self.data.__repr__()})'
```

В этом примере доступ к атрибуту `data` позволяет создать класс `StringList` более простым способом. При этом функция `super()` используется один раз в инициализаторе класса `__init__()` для предотвращения проблемы в дальнейших сценариях наследования. В остальных методах используется атрибут `data`, который содержит обычный список.

Приведенный ниже код:

```python
lst = StringList([1, 2, 3])

lst[2] = 4
lst.append(5)
lst.insert(0, 6)
lst.extend([6, 7, 8])

print(lst)
```

выводит:

```python
StringList(['6', '1', '2', '4', '5', '6', '7', '8'])
```

Если есть необходимость, чтобы класс `StringList` поддерживал конкатенацию с помощью оператора плюс `+`, то потребуется реализовать магические методы `__add__()`, `__radd__()` и `__iadd__()`.