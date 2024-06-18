

Метод `new_child(m=None)` возвращает **новый объект** `ChainMap()`, содержащий новый словарь `m`, за которым следуют все словари текущего объекта:

- если указан словарь `m`, то он вставляется первым в списке существующих словарей текущего объекта `ChainMap`
- если `m` не указан, то используется пустой словарь, который также вставляется первым

Приведенный ниже код:

```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}

old_family = ChainMap(dad, mom)

son = {'name': 'Soslan', 'age': 0}

new_family = old_family.new_child(son)

print(old_family)
print(new_family)
```

выводит:

```no-highlight
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({'name': 'Soslan', 'age': 0}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
```

Приведенный ниже код:

```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}

old_family = ChainMap(dad, mom)
new_family = old_family.new_child()

print(old_family)
print(new_family)
```

выводит:

```no-highlight
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
```

  Обратите внимание, что метод `new_child()` не обновляет старый объект `ChainMap`, а создаёт и возвращает новый.
  
Вызов метода `d.new_child()` эквивалентен вызову `ChainMap({}, *d.maps)`.