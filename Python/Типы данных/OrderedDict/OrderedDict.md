

Тип `OrderedDict` является подтипом типа [[Тип данных dict]], сохраняющий порядок, в котором пары "ключ-значение" вставляются в словарь. Когда мы перебираем объект типа `OrderedDict`, его элементы перебираются в исходном порядке. Если мы обновим значение существующего ключа, то порядок останется неизменным. Если мы удалим элемент и вставим его снова, то этот элемент будет добавлен в конец словаря.

Тип `OrderedDict` будучи подтипом `dict` наследует все методы, предоставляемые обычным словарем. При этом в `OrderedDict` также есть дополнительные методы, о которых мы поговорим ниже.

В отличие от `dict`, тип `OrderedDict` не является встроенным типом и для использования его необходимо импортировать из модуля `collections`.

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict()

numbers['one'] = 1
numbers['two'] = 2
numbers['three'] = 3

print(numbers)
```

выводит:

```no-highlight
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
```


 В Python 3.9 появились операторы `|` и `|=` которые реализуют операцию конкатенации словарей, как обычных, так и `OrderedDict`.

Приведенный ниже код:

```python
from collections import OrderedDict

physicists = OrderedDict(newton='1642-1726', einstein='1879-1955')
biologists = OrderedDict(darwin='1809-1882', mendel='1822-1884')

scientists = physicists | biologists
print(scientists)
```

выводит:

```no-highlight
OrderedDict([('newton', '1642-1726'), ('einstein', '1879-1955'), ('darwin', '1809-1882'), ('mendel', '1822-1884')])
```

Приведенный ниже код:

```python
from collections import OrderedDict

physicists = OrderedDict(newton='1642-1726', einstein='1879-1955')
physicists1 = OrderedDict(newton='1642-1726/1727', hawking='1942-2018')

physicists |= physicists1

print(physicists)
```

выводит:

```no-highlight
OrderedDict([('newton', '1642-1726/1727'), ('einstein', '1879-1955'), ('hawking', '1942-2018')])
```

Тип данных `OrderedDict` написан на языке C и реализован в виде двусвязного списка для сохранения порядка элементов.

`OrderedDict` словари содержат дополнительный [[Атрибут __dict__]] которого нет у обычного словаря. Данный атрибут используется для динамического наделения объектов дополнительным функционалом.

Приведенный ниже код:

```python
from collections import OrderedDict

letters = OrderedDict(b=2, d=4, a=1, c=3)
print(letters)
print(letters.__dict__)

letters.__dict__['advanced'] = '144'
print(letters)
print(letters.__dict__)
```

выводит:

```no-highlight
OrderedDict([('b', 2), ('d', 4), ('a', 1), ('c', 3)])
{}
OrderedDict([('b', 2), ('d', 4), ('a', 1), ('c', 3)])
{'advanced': '144'}
```

Приведенный ниже код:

```python
letters = dict(b=2, d=4, a=1, c=3)

print(letters.__dict__)    # у обычных словарей нет атрибута __dict__
```

приводит к возникновению ошибки:

```no-highlight
AttributeError: 'dict' object has no attribute '__dict__'
```

Для динамического задания новых атрибутов мы можем использовать два синтаксиса:

- в стиле словаря: `ordered_dict.__dict__['attr'] = value`
- через точечную нотацию: `ordered_dict.attr = value`

Приведенный ниже код:

```python
from collections import OrderedDict

letters = OrderedDict(b=2, d=4, a=1, c=3)

letters.sorted_keys = lambda: sorted(letters.keys())

print(letters)
print(letters.sorted_keys())

letters['e'] = 5
print(letters)
print(letters.sorted_keys())

for key in letters.sorted_keys():
    print(key, '->', letters[key])
```

выводит:

```no-highlight
OrderedDict([('b', 2), ('d', 4), ('a', 1), ('c', 3)])
['a', 'b', 'c', 'd']
OrderedDict([('b', 2), ('d', 4), ('a', 1), ('c', 3), ('e', 5)])
['a', 'b', 'c', 'd', 'e']
a -> 1
b -> 2
c -> 3
d -> 4
e -> 5
```

Таким образом, мы наделили  `OrderedDict` словарь (переменная `letters`) дополнительным методом (атрибутом) `sorted_keys()`, который возвращает список упорядоченных ключей.