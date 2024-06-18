

Методу `move_to_end()` можно передать два аргумента:

- `key` (обязательный аргумент) – ключ, который идентифицирует перемещаемый элемент
    
- `last` (необязательный аргумент) – логическое значение (тип `bool`), которое определяет, в какой конец словаря мы перемещаем элемент, значение `True` (по умолчанию) перемещает элемент в конец, значение `False` – в начало
    

 Если при вызове метода `move_to_end()` переданный ключ отсутствует в словаре, то возникает ошибка `KeyError`.

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

numbers.move_to_end('one')       # last=True
print(numbers)

numbers.move_to_end('three', last=False)       # last=False
print(numbers)
```

выводит:

```no-highlight
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('two', 2), ('three', 3), ('one', 1)])
OrderedDict([('three', 3), ('two', 2), ('one', 1)])
```

С помощью метода `move_to_end()` мы можем сортировать `OrderedDict` словарь по ключам.

Приведенный ниже код:

```python
from collections import OrderedDict

letters = OrderedDict(b=2, d=4, a=1, c=3)

for key in sorted(letters):
    letters.move_to_end(key)

print(letters)
```

выводит:

```no-highlight
OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])
```
