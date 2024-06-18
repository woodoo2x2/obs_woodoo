

Для изменения объектов типа `ChainMap` мы можем использовать те же способы, что и для изменения обычного словаря ([[Тип данных dict]]). Мы можем обновлять, добавлять, удалять и извлекать элементы из `ChainMap` объекта. При этом нужно знать, что все эти операции действуют только на первый из объединяемых словарей.

Приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)
print(alpha_num)

alpha_num['c'] = 'C'
print(alpha_num)

alpha_num['b'] = 'b'
print(alpha_num)

alpha_num.pop('two')
print(alpha_num)

del alpha_num['c']
print(alpha_num)

alpha_num.clear()
print(alpha_num)
```

выводит:

```no-highlight
ChainMap({'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'two': 2, 'c': 'C'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'two': 2, 'c': 'C', 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'c': 'C', 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({}, {'a': 'A', 'b': 'B'})
```

При попытке удаления значения по ключу, которого нет в первом словаре, возникает ошибка `KeyError`, даже если указанный ключ есть в одном из объединяемых словарей, кроме первого.

Поскольку все изменения `ChainMap` объекта действуют только на первый из объединяемых словарей, то приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap({}, numbers, letters)
print(alpha_num)

alpha_num['colon'] = ':'
alpha_num['comma'] = ','
alpha_num['period'] = '.'
print(alpha_num)
```

выводит:

```no-highlight
ChainMap({}, {'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
ChainMap({'colon': ':', 'comma': ',', 'period': '.'}, {'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
```

Таким образом, указывая в качестве первого аргумента для `ChainMap` пустой словарь, мы получаем поведение, при котором все изменения `ChainMap` объекта не затрагивают объединяемые (исходные) словари.

Изменение содержания любого словаря, на основании которого создан `ChainMap`, изменяет и сам `ChainMap` объект.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)
print(pets)

for_adoption['dogs'] = 10000
for_adoption['lions'] = 9

print(pets)
```

выводит:

```no-highlight
ChainMap({'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
ChainMap({'dogs': 10000, 'cats': 8, 'pythons': 9, 'lions': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
```

Аналогично, изменение `ChainMap` объекта приводит к изменению словаря, на основании которого он создан.

Обратите внимание на то, что `ChainMap` может обновлять (изменять, вставлять и удалять) значения ключей только в первом словаре, в то время как поиск ключей осуществляется по всем словарям в списке.