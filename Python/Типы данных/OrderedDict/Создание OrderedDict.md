Как и [[!Тип данных collections.defaultdict]], эти словари можно создавать любым из доступных способов, как и обычные словари:

```python
from collections import OrderedDict

numbers1 = OrderedDict({'one': 1, 'two': 2, 'three': 3})
numbers2 = OrderedDict([('one', 1), ('two', 2), ('three', 3)])
numbers3 = OrderedDict(one=1, two=2, three=3)
```

Мы можем использовать метод `fromkeys()` для создания `OrderedDict` словарей.

Приведенный ниже код:

```python
from collections import OrderedDict

keys = ['one', 'two', 'three']
numbers = OrderedDict.fromkeys(keys, 0)
print(numbers)
```

выводит:

```no-highlight
OrderedDict([('one', 0), ('two', 0), ('three', 0)])
```
