Создание ChainMap объекта

Объекты типа `ChainMap` обычно создаются на основе словарей.

Приведенный ниже код:

```python
from collections import ChainMap

empty_chain_map = ChainMap()                      # создаем пустой ChainMap объект
print(empty_chain_map)

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

chain_map = ChainMap(numbers, letters)            # создаем ChainMap объект на основе словарей numbers и letters
print(chain_map)
```

выводит:

```no-highlight
ChainMap({})
ChainMap({'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
```

Мы также можем создавать объекты типа `ChainMap`, используя метод [[Метод fromkeys()(dict)]].

Приведенный ниже код:

```python
from collections import ChainMap

chain_map1 = ChainMap.fromkeys(['one', 'two', 'three'])
chain_map2 = ChainMap.fromkeys(['one', 'two', 'three'], -1)

print(chain_map1)
print(chain_map2)
```

выводит:

```no-highlight
ChainMap({'one': None, 'two': None, 'three': None})
ChainMap({'one': -1, 'two': -1, 'three': -1})
```
Нам ничего не мешает использовать любой из ранее изученных словарей: [[!Тип данных collections.defaultdict]], [[Тип данных collections.OrderedDict]], [[Тип данных collections.Counter]].

```python
from collections import defaultdict, OrderedDict, Counter, ChainMap

numbers = OrderedDict(one=1, two=2)
letters = defaultdict(str, {'a': 'A', 'b': 'B'})
counter = Counter('aabbbcccc')

chain_map = ChainMap(numbers, letters, counter)

print(chain_map)
```

выводит:

```no-highlight
ChainMap(OrderedDict([('one', 1), ('two', 2)]), defaultdict(<class 'str'>, {'a': 'A', 'b': 'B'}), Counter({'c': 4, 'b': 3, 'a': 2}))
```

При этом нужно понимать, что поиск по `ChainMap` объекту будет учитывать особенность поиска по соответствующим словарям. Для `defaultdict`, в случае если ключ отсутствует, мы будем получать значение по умолчанию, для `Counter` – нулевое значение.

При создании пустого `ChainMap` объекта его атрибут [[Атрибут maps (ChainMap)]] будет содержать пустой словарь.

Приведенный ниже код:

```python
from collections import ChainMap

empty_chain = ChainMap()

print(empty_chain.maps)
```

выводит:

```no-highlight
[{}]
```
