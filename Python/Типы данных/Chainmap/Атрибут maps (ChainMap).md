

Объект `ChainMap` хранит все объединяемые словари во внутреннем списке. Этот список доступен через атрибут `maps` и может быть изменен. Порядок словарей в списке `maps` соответствует порядку, в котором словари были указаны при создании объекта `ChainMap`.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(pets)
print(pets.maps)
print(type(pets.maps))
```

выводит:

```no-highlight
ChainMap({'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
[{'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3}]
<class 'list'>
```

Атрибут `maps` является обычным списком, поэтому он поддерживает все основные операции со списками. Мы можем добавлять в него новые словари, удалять уже добавленные, а также изменять их порядок.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

pets.maps.reverse()
pets.maps[0]['lions'] = 10
del pets.maps[1]['cats']

print(pets)
print(pets.maps)
```

выводит:

```no-highlight
ChainMap({'dogs': 7, 'cats': 2, 'tigers': 3, 'lions': 10}, {'dogs': 15, 'pythons': 9})
[{'dogs': 7, 'cats': 2, 'tigers': 3, 'lions': 10}, {'dogs': 15, 'pythons': 9}]
```

Обратите внимание, что изменяя порядок словарей в списке атрибута `maps`, мы также меняем сами объединяемые словари, а также порядок поиска в объекте `ChainMap`.

Атрибут `maps` можно использовать для обработки абсолютно всех значений во всех словарях. С помощью этого атрибута мы можем обойти поведение по умолчанию, заключающееся в получении (изменении) первого значения из первого словаря.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for animals in pets.maps:
    for key, value in animals.items():
        print(key, '->', value)
```

выводит:

```no-highlight
dogs -> 15
cats -> 8
pythons -> 9
dogs -> 7
cats -> 2
tigers -> 3
```


