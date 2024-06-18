

Существует несколько способов создания объектов типа `Counter`. Самый простой и распространенный способ основан на передаче коллекции с данными (список, строка, кортеж и т.д.) в конструктор типа.

Приведенный ниже код:

```python
from collections import Counter

counter = Counter('mississippi')     # создаем счетчик на основе строки
print(counter)
```

выводит:

```no-highlight
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```

  Мы можем преобразовать объект типа `Counter` в обычный словарь с помощью  [[Функция dict()]].

Мы можем создавать объекты типа `Counter`, явно указывая начальные значения количества объектов.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
counter2 = Counter(i=4, s=4, p=2, m=1)

print(counter1)
print(counter2)
```

выводит:

```no-highlight
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```

Тип `Counter`, будучи подтипом типа `dict`, наследует все методы, предоставляемые обычным словарем. При этом вызов [[Метод fromkeys()(dict)]] всегда будет приводить к возникновению ошибки:

```no-highlight
NotImplementedError: Counter.fromkeys() is undefined.  Use Counter(iterable) instead.
```

Такое поведение не случайно, оно позволяет избежать ошибок неоднозначности при создании объектов типа `Counter`.

Например, код:

```python
from collections import Counter

counter = Counter.fromkeys('mississippi', 2)
```

мог бы создать объект типа `Counter` на основе строки `mississippi` со значением по умолчанию равным 22 для всех символов строки, несмотря на реальное количество вхождений символов в строке `mississippi`.

Тип данных `Counter` накладывает такие же ограничения на ключи, как и обычные словари (тип `dict`): ключи должны быть хешируемыми. При этом нет никаких ограничений на объекты, которые мы будем хранить в качестве значений.

Приведенный ниже код:

```python
from collections import Counter

inventory = Counter(apple=5, orange=7, banana=0, tomato=-2, watermelon='N/A')

print(inventory)
```

выводит:

```no-highlight
Counter({'apple': 5, 'orange': 7, 'banana': 0, 'tomato': -2, 'watermelon': 'N/A'})
```

Тип `Counter` позволяет нам писать такой код, однако нужно понимать, что для правильной работы в качестве подсчета объектов значения должны быть целыми неотрицательными числами, представляющими количество.
