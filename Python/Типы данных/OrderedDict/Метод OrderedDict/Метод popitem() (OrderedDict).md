

Метод `popitem()` по умолчанию удаляет и возвращает элемент в порядке [LIFO](https://ru.wikipedia.org/wiki/LIFO) (Last-In/First-Out, последний пришел/первый ушел). Другими словами, метод `popitem()` удаляет элементы с конца словаря.

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

print(numbers.popitem())
print(numbers)

print(numbers.popitem())
print(numbers)
```

выводит:

```no-highlight
('three', 3)
OrderedDict([('one', 1), ('two', 2)])
('two', 2)
OrderedDict([('one', 1)])
```

Если методу `popitem()` передать необязательный аргумент `last=False`, то он начнет удалять и возвращать элементы в порядке [FIFO](https://ru.wikipedia.org/wiki/FIFO) (First-In/First-Out, первый пришел/первый ушел).

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

print(numbers.popitem(last=False))
print(numbers)

print(numbers.popitem(last=False))
print(numbers)
```

выводит:

```no-highlight
('one', 1)
OrderedDict([('two', 2), ('three', 3)])
('two', 2)
OrderedDict([('three', 3)])
```
