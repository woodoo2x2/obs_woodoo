

Мы можем преобразовывать именованные кортежи в словари с помощью метода `_asdict()`. Этот метод возвращает словарь, в котором имена полей используются в качестве ключей. Ключи результирующего словаря находятся в том же порядке, что и поля в исходном именованном кортеже.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person._make(['Timur', 29, 170])

print(timur._asdict())
```

выводит:

```no-highlight
{'name': 'Timur', 'age': 29, 'height': 170}
```

До Python 3.8 метод `_asdict()` возвращал тип данных `OrderedDict`. В настоящий момент метод возвращает обычный словарь (тип `dict`), так как сейчас словари запоминают порядок добавления в них ключей.