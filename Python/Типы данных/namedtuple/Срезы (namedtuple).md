При работе с именованным кортежами мы можем использовать срезы.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point3D', ['x', 'y', 'z'])

point = Point(89, 54, -34)

print(point[1:])
print(point[:2])
print(point[1:2])
```

выводит:

```no-highlight
(54, -34)
(89, 54)
(54,)
```

Обратите внимание на то, что результатом среза является обычный кортеж (тип данных [[Тип данных tuple]]).