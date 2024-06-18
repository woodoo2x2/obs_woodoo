Операторы +, -, &, |

Как мы уже знаем, методы [[Метод update() (dict)]] и [[Метод subtract() (collections.Counter)]] объединяют `Counter` словари путем сложения и вычитания количества соответствующих элементов. Python предоставляет удобные операторы сложения (`+`) и вычитания (`-`), которые могут заменить вызовы данных методов.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=10, s=40, p=10, m=1)
counter2 = Counter(i=2, s=8, p=10, m=3)

print(counter1 + counter2)
print(counter1 - counter2)
print(counter2 - counter1)
```

выводит:

```no-highlight
Counter({'s': 48, 'p': 20, 'i': 12, 'm': 4})
Counter({'s': 32, 'i': 8})
Counter({'m': 2})
```

Обратите внимание на то, что при использовании операторов `+` и `-` из результирующего словаря исключаются элементы с нулевыми и отрицательными значениями.

Помимо указанных выше операторов, Python также предоставляет операторы пересечения (`&`) и объединения (`|`), которые возвращают минимум и максимум из соответствующих значений.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=10, s=40, p=10, m=1)
counter2 = Counter(i=2, s=8, p=10, m=3)

print(counter1 & counter2)
print(counter1 | counter2)
```

выводит:

```no-highlight
Counter({'p': 10, 's': 8, 'i': 2, 'm': 1})
Counter({'s': 40, 'i': 10, 'p': 10, 'm': 3})
```

Операторы сложения (`+`) и вычитания (`-`) работают только с `Counter` словарями, в то время как методы `update()` и `subtract()` с любым итерируемым объектом: списком, строкой, кортежем и т.д.


 Тип `Counter` позволяет также использовать унарные операторы сложения (`+`) и вычитания (`-`).

Приведенный ниже код:

```python
from collections import Counter

counter = Counter(a=5, b=-9, c=0)

print(+counter)
print(-counter)
```

выводит:

```no-highlight
Counter({'a': 5})
Counter({'b': 9})
```

Когда мы используем оператор сложения (`+`) в качестве унарного, мы получаем новый `Counter` словарь, содержащий элементы, значения которых больше нуля. Аналогично, при использовании унарного оператора (`-`), мы получаем новый `Counter` словарь, содержащий элементы, значения которых меньше нуля.

Другими словами, операторы унарного сложения и вычитания прибавляют пустой `Counter` словарь или вычитают исходный из пустого.

Приведенный выше код, равнозначен коду:

```python
from collections import Counter

counter = Counter(a=5, b=-9, c=0)

print(Counter() + counter)
print(Counter() - counter)
```

 С версии Python 3.10 тип `Counter` поддерживает расширенные операторы сравнения для отношений **подмножества и надмножества**: `<`, `<=`, `>`, `>=`. Все эти сравнения рассматривают отсутствующие элементы как имеющие нулевое количество.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter('aabc')
counter2 = Counter('abc')

print(counter1 > counter2)

counter1 = Counter('abcde')
counter2 = Counter('abc')

print(counter1 > counter2)

counter1 = Counter('abcde')
counter2 = Counter('abcdf')

print(counter1 > counter2)
print(counter1 < counter2)
```

выводит:

```no-highlight
True
True
False
False
```

