Для создания [[Свойства ООП|свойств]] используется встроенная функция . Она принимает четыре аргумента:

- `fget` — функция для получения значения атрибута
- `fset` — функция для установки значения атрибута
- `fdel` — функция для удаления атрибута
- `doc` — строка документации

Функция `property()` возвращает специальный объект `property` — свойство на основе переданных [[Геттеры|геттера,]] [[Сеттеры|сеттера]] и [[Делитеры|делитера]].

 Все четыре аргумента функции `property()` являются необязательными и по умолчанию имеют значение `None`.

Третьим аргументом, передаваемым в функцию `property()`, является делитер. Он вызывается во время удаления свойства как атрибута.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def del_name(self):
        del self._name

    name = property(get_name, fdel=del_name)


cat = Cat('Кемаль')

del cat.name                                           # равнозначно cat.del_name()

print(cat.name)
```

приводит к возбуждению исключения:

```no-highlight
AttributeError: 'Cat' object has no attribute '_name'.
```

Четвертым аргументом, передаваемым в функцию `property()` является строка документации, которая может использоваться для описания создаваемого свойства.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def del_name(self):
        del self._name

    name = property(get_name, doc='Имя кошки. Доступно только для чтения.')


cat = Cat('Кемаль')

print(Cat.name.__doc__)
```

выводит:

```no-highlight
Имя кошки. Доступно только для чтения.
```