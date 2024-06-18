

Итерирование по `ChainMap` объекту происходит **в обратном порядке** от последнего указанного словаря к первому.

Приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)

for key in alpha_num:
    print(key, '->', alpha_num[key])
```

выводит:

```no-highlight
a -> A
b -> B
one -> 1
two -> 2
```

При этом если в `ChainMap` объекте есть повторяющиеся ключи в объединяемых словарях, то мы будем получать первое из значений.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for key in pets:
    print(key, '->', pets[key])
```

выводит: 

```no-highlight
dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9
```

При итерировании по `ChainMap` объекту мы можем использовать методы [[Метод keys()(dict)]], [[Метод values()(dict)]], [[Метод items()(dict)]].

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for key in pets.keys():
    print(key, '->', pets[key])

print()

for value in pets.values():
    print(value)

print()

for key, value in pets.items():
    print(key, '->', value)
```

выводит:

```no-highlight
dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9

15
8
3
9

dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9