В качестве альтернативы объединению нескольких словарей с помощью `ChainMap` мы можем объединять их с помощью словарного метода [[Метод update() (dict)]].

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'hamsters': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)
```

можно переписать так:

```python
for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'hamsters': 2, 'tigers': 3}

pets = {}
pets.update(for_adoption)
pets.update(vet_treatment)
```

В приведенном конкретно выше примере поиск по результирующему объекту `pets` будет работать одинаково. Объединение словарей с помощью метода `update()` имеет существенный недостаток по сравнению с объединением их с помощью `ChainMap`. Недостаток заключается в том, что мы теряем возможность управлять доступом к повторяющимся ключам. При использовании метода `update()` новые значения перезаписывают старые.

Приведенный ниже код:

```python
for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = {}
pets.update(for_adoption)
pets.update(vet_treatment)

print(pets)
```

выводит:

```no-highlight
{'dogs': 7, 'cats': 2, 'pythons': 9, 'tigers': 3}
```

В Python 3.3 в модуль `collections` добавили новый тип данных `ChainMap`, который представляет из себя объединение нескольких словарей. Этот объект группирует несколько словарей вместе, что позволяет рассматривать их как единое целое.

Встроенная [[Функция len()]] возвращает количество уникальных ключей `ChainMap` объекта.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(len(pets))
```

выводит:

```no-highlight
4
```

Тип данных `ChainMap` удобен в том случае, когда мы уже имеем некоторую коллекцию с большим количеством словарей и нам требуется производить поиск по всем словарям одновременно.

Приведенный ниже код:

```python
from collections import ChainMap

dicts = [{'math': 4, 'physics': 5, 'geometry': 4},
         {'biology': 3, 'chemistry': 3},
         {'literature': 4, 'languages': 4},
         {'history': 3, 'music': 3}]

lessons = ChainMap(*dicts)

print(lessons['literature'])
```

выводит:

```python
4
```