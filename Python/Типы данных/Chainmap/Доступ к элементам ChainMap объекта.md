

Для получения значений по ключу в `ChainMap` объектах используется такой же механизм, как и в обычных словарях ([[Тип данных dict]]). Либо мы используем квадратные скобки, либо метод [[Метод get()(dict)]].

Приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)


print(alpha_num['one'])
print(alpha_num['b'])
print(alpha_num.get('a'))
print(alpha_num.get('c'))
print(alpha_num.get('d', False))
```

выводит:

```no-highlight
1
B
A
None
False
```

Рассмотрим `ChainMap` объект, в котором есть повторяющиеся ключи в объединяемых словарях.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(pets['dogs'])
print(pets['cats'])

print(pets['pythons'])
print(pets['tigers'])
```

выводит: 

```no-highlight
15
8
9
3
```

Как мы видим, в ситуации, когда у объединяемых словарей есть повторяющиеся ключи, мы получаем только значение из первого словаря, в котором встречается этот ключ. Таким образом, поиск по `ChainMap` объекту всегда осуществляется в том же порядке, в котором словари были указаны при создании `ChainMap` объекта, при этом поиск останавливается, как только значение по нужному ключу найдено.