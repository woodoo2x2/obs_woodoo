

Тип `OrderedDict` является изменяемым. Мы можем вставлять новые элементы, обновлять и удалять существующие элементы. Если мы вставим новый элемент в существующий `OrderedDict` словарь, то этот элемент добавится в конец словаря.

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

print(numbers)

numbers['four'] = 4

print(numbers)
```

выводит:

```no-highlight
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('one', 1), ('two', 2), ('three', 3), ('four', 4)])
```

Если мы удалим элемент из существующего `OrderedDict` словаря и снова вставим его, то он будет помещен в конец словаря.

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

del numbers['one']

print(numbers)
numbers['one'] = 1
print(numbers)
```

выводит:

```no-highlight
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('two', 2), ('three', 3)])
OrderedDict([('two', 2), ('three', 3), ('one', 1)])
```

Если мы обновляем значение по существующему ключу, то ключ сохраняет свою позицию.

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

numbers['one'] = 1.0
print(numbers)

numbers.update(two=2.0)
print(numbers)
```

выводит:

```no-highlight
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('one', 1.0), ('two', 2), ('three', 3)])
OrderedDict([('one', 1.0), ('two', 2.0), ('three', 3)])
```
Обновить значение по нужному ключу можно либо с помощью квадратных скобок, либо с помощью словарного метода [[Метод update() (dict)]].
