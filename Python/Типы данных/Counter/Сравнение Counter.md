Объекты типа `Counter` можно сравнивать между собой. Равные объекты имеют одинаковое количество элементов и содержат равные элементы (`ключ: количество`). Для сравнения используются операторы `==` и `!=`.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
counter2 = Counter(m=1, s=4, i=4, p=2)
counter3 = Counter('iiiisspm')

print(counter1 == counter2)
print(counter1 == counter3)
```

выводит:

```no-highlight
True
False
```

До версии Python 3.10 словари `Counter(i=4)` и `Counter(i=4, s=0)` считались разными. Начиная с Python 3.10 сравнение рассматривает отсутствующие элементы как имеющие нулевое значение, поэтому приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=4)
counter2 = Counter(i=4, s=0)

print(counter1 == counter2)
```

выводит:

```no-highlight
True
```