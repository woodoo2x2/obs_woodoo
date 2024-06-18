

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
point = Point(1, 2)
print(type(point))
```

выводит:

```no-highlight
<class '__main__.Point'>
```

Если мы укажем допустимое имя модуля для этого аргумента, тогда атрибуту `.__ module__` результирующего именованного кортежа будет присвоено это значение.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], module='customtypes')
point = Point(1, 2)
print(type(point))
```

выводит:

```no-highlight
<class 'customtypes.Point'>
```

Параметр `module` был добавлен в Python 3.6 для того, чтобы появилась возможность сериализовать/десериализовать именованные кортежи с помощью модуля `pickle` в разных реализациях Python (IronPython, Jython и т.д.)