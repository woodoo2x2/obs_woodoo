Обратите внимание на то, как выводятся именованные кортежи: сначала выводится имя, а затем поля и их значения в том порядке, в котором они были указаны при создании.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])

point = Point(1, 5)

print(point)
```

выводит: 

```no-highlight
Point(x=1, y=5)
```