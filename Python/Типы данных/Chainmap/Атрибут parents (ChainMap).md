
Атрибут `parents` возвращает новый объект `ChainMap`, содержащий все словари, кроме первого. Это полезно для пропуска первого словаря при поиске ключей.

Приведенный ниже код:

```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}
son = {'name': 'Soslan', 'age': 0}

family = ChainMap(son, dad, mom)

print(family)
print(family.parents)
print(type(family.parents))
```

выводит:

```no-highlight
ChainMap({'name': 'Soslan', 'age': 0}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
<class 'collections.ChainMap'>
```

В некотором смысле атрибут `parents` выполняет обратную функцию методу [[Метод new_child() (ChainMap)]]. Первый удаляет словарь, а второй добавляет новый словарь в начало списка. В обоих случаях вы получаете новый объект `ChainMap`.

Обращение к атрибуту `d.parents` эквивалентно вызову `ChainMap(*d.maps[1:])`.