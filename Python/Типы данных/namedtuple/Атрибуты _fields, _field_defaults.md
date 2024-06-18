

Именованные кортежи имеют два дополнительных атрибута: `_fields` и `_field_defaults`. Первый атрибут содержит кортеж строк, в котором перечислены имена полей. Второй атрибут содержит словарь, который сопоставляет имена полей с соответствующими значениями по умолчанию, если таковые имеются.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

tim = Person('Тимур', 29, 170)

print(tim)
print(tim._fields)
print(Person._fields)
```

выводит:

```no-highlight
Person(name='Тимур', age=29, height=170)
('name', 'age', 'height')
('name', 'age', 'height')
```
Обратите внимание на то, что мы можем обращаться к атрибуту `_fields` как через переменную (`tim`), так и через сам тип именованного кортежа (`Person`).

С помощью атрибута `_fields` мы можем создавать новые именованные кортежи на основании уже существующих. В следующем примере мы создадим новый именованный кортеж с именем `ExtendedPerson`, который расширяет старый `Person` новым полем `weight`.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

ExtendedPerson = namedtuple('ExtendedPerson', [*Person._fields, 'weight'])  # распаковка полей старого кортежа

timur = ExtendedPerson('Тимур', 29, 170, 65)

print(timur)
print(ExtendedPerson._fields)
```

выводит:

```no-highlight
ExtendedPerson(name='Тимур', age=29, height=170, weight=65)
('name', 'age', 'height', 'weight')
```

Мы также можем использовать атрибут `_fields` для перебора полей и их значений с помощью встроенной функции `zip()`:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person('Тимур', 29, 170)

for field, value in zip(Person._fields, timur):
    print(field, '->', value)
```

выводит:

```no-highlight
name -> Тимур
age -> 29
height -> 170
```

С помощью атрибута `_field_defaults` мы можем выяснить, какие поля именованного кортежа имеют значения по умолчанию. Значения по умолчанию делают поля необязательными. Например, предположим, что наш именованный кортеж `Person` должен включать дополнительное поле для хранения страны, в которой живет человек. Поскольку в основном мы работаем с людьми из России, то мы устанавливаем соответствующее значение по умолчанию для поля страны следующим образом:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'], defaults=['Russia'])

timur = Person('Тимур', 29, 170)

print(timur)
print(timur._field_defaults)
print(Person._field_defaults)
```

Приведенный выше код выводит:

```no-highlight
Person(name='Тимур', age=29, height=170, country='Russia')
{'country': 'Russia'}
{'country': 'Russia'}
```

Если именованный кортеж не предоставляет значений по умолчанию, тогда атрибут `_field_defaults` содержит пустой словарь.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'])

timur = Person('Тимур', 29, 170, 'Russia')

print(Person._field_defaults)
```

выводит:

```no-highlight
{}
```