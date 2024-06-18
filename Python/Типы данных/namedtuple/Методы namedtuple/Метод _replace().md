

Метод `_replace()` позволяет создавать новые именованные кортежи на основании уже существующих с заменой некоторых значений. Потребность в данном методе вызвана тем, что именованные кортежи являются неизменяемыми.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'])

timur1 = Person('Тимур', 29, 170, 'Russia')
timur2 = timur1._replace(age=30, country='Germany')

print(timur1)
print(timur2)
```

выводит:

```no-highlight
Person(name='Тимур', age=29, height=170, country='Russia')
Person(name='Тимур', age=30, height=170, country='Germany')
```
 Обратите внимание на то, что метод `_replace()` не изменяет текущий именованный кортеж, а возвращает новый.