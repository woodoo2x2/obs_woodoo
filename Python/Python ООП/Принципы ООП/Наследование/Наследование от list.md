
Аналогичные проблемы мы получаем при наследовании от типа [[Тип данных list]]. Создадим класс `StringList`, наследника класса `list`, который автоматически сохраняет все свои элементы в виде строк.

Для реализации класса `StringList` мы переопределим магические методы `__init__(), __setitem__()` и для удобства `__repr__()`:

```python
class StringList(list):
    def __init__(self, iterable):
        super().__init__(str(item) for item in iterable)

    def __setitem__(self, index, item):
        super().__setitem__(index, str(item))

    def __repr__(self):
        return f'{type(self).__name__}({super().__repr__()})'
```

Приведенный ниже код:

```python
li = StringList([1, 2, 3])
li[2] = 4

print(li)
```

выводит:

```no-highlight
StringList(['1', '2', '4'])
```

Как мы видим, инициализация и установка элементов работает так, как мы и ожидаем.

Однако если мы попытаемся добавить в список новый элемент с помощью методов `append(), insert()` или `extend()`, то получим не совсем ожидаемое поведение.

Приведенный ниже код:

```python
li = StringList([1, 2, 3])
li.append(4)
li.insert(0, 5)
li.extend([6, 7, 8])

print(li)
```

выводит:

```no-highlight
StringList([5, '1', '2', '3', 4, 6, 7, 8])
```

Мы можем исправить это поведение с помощью переопределения методов  `append(), insert()` и `extend()`:

```python
class StringList(list):
    def __init__(self, iterable):
        super().__init__(str(item) for item in iterable)

    def __setitem__(self, index, item):
        super().__setitem__(index, str(item))

    def insert(self, index, item):
        super().insert(index, str(item))

    def append(self, item):
        super().append(str(item))

    def extend(self, other):
        if isinstance(other, type(self)):
            super().extend(other)
        else:
            super().extend(str(item) for item in other)

    def __repr__(self):
        return f'{type(self).__name__}({super().__repr__()})'
```

Приведенный ниже код:

```python
li = StringList([1, 2, 3])
li.append(4)
li.insert(0, 5)
li.extend([6, 7, 8])

print(li)
```

выводит:

```no-highlight
StringList(['5', '1', '2', '3', '4', '6', '7', '8'])
```

И опять, как мы видим, для правильной работы класса `StringList` необходимо переопределить много методов базового типа `list`.

Методы `remove()` и `pop()` типа `list` не вызывают магический метод `__delitem__()`. Метод `__ne__()` не вызывает метод `__eq__()`.

**Подводя итог**: каждый раз, когда мы настраиваем функционал в наследниках классов `dict` или `list`, нам нужно убедиться, что мы переопределяем все методы, затрагивающие этот функционал.


## Наследование от типов list и dict не всегда плохо

Может сложиться впечатление, что наследование от типов `list` и `dict` – это всегда плохая идея и что всегда нужно использовать типы `UserList` и `UserDict`. Однако это не так. В ситуациях, когда мы изменяем функциональность, ограниченную одним методом, или добавляем собственные методы, никаких проблем не возникает.

Создадим класс `DefaultDict`, наследника класса `dict`, который возвращает значение по умолчанию в ситуации, когда нужного ключа нет в словаре:

```python
class DefaultDict(dict):
    def __init__(self, *args, default=None, **kwargs):
        super().__init__(*args, **kwargs)
        self.default = default
    
    def __missing__(self, key):
        return self.default
```

Приведенный ниже код:

```python
dd1 = DefaultDict({'one': 1})
print(dd1['one'])
print(dd1['two'])

dd2 = DefaultDict({'one': 1}, default='empty result')
print(dd2['one'])
print(dd2['two'])
```

выводит:

```no-highlight
1
None
1
empty result
```

В классе `DefaultDict` нет никаких проблем с наследованием от `dict`, потому что нам требуется переопределить всего один магический метод `__missing__()`. Возвращаемое значение метода `__missing__()` – это значение, которое будет возвращено при попытке получить значение по несуществующему ключу. По умолчанию метод `__missing__()` возбуждает исключение `KeyError`.
 
 Метод `__getitem__()` внутренне вызывает метод `__missing__()`, если ключ не существует.