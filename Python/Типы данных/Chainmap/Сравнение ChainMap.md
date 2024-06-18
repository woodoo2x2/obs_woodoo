 Два объекта типа `ChainMap` `chainmap1` и `chainmap2` считаются равными, если значение следующего выражения равно `True`:

```python
dict(chainmap1.items()) == dict(chainmap2.items())
```

Учитывая специфику работы метода `items()`, равенство двух объектов типа `ChainMap` не гарантирует того, что эти объекты в точности совпадают.

Приведенный ниже код:

```python
from collections import ChainMap

chainmap1 = ChainMap({'a': 10, 'b': 20})
chainmap2 = ChainMap({'a': 10, 'b': 20})

print(chainmap1 == chainmap2)

chainmap1 = ChainMap({'a': 10, 'b': 20}, {'a': 1, 'b': 2})
chainmap2 = ChainMap({'a': 10, 'b': 20})

print(chainmap1 == chainmap2)
```

выводит:

```no-highlight
True
True
```
