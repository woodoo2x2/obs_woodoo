

Метод `subtract()` вычитает из значений элементов одного словаря `Counter` значения элементов другого словаря. Метод `subtract()` подобен методу [[Метод update() (dict)]], но вычитает количества, а не складывает их. При этом у результирующего словаря значения ключей могут быть нулевыми или отрицательными.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=4, s=40, a=1, p=20, b=98, z=69)
counter2 = Counter(i=2, s=20, a=6, p=12, m=1, z=69)

counter1.subtract(counter2)       # обновляем значения в counter1

print(counter1)
```

выводит:

```no-highlight
Counter({'b': 98, 's': 20, 'p': 8, 'i': 2, 'z': 0, 'm': -1, 'a': -5})
```

Мы можем использовать метод `subtract()` с именованными аргументами. Вызов метода `counter1.subtract(i=2, s=20, a=6, p=12, m=1, z=69)` равнозначен вызову метода `counter1.subtract(counter2)`.

Помимо словарей, метод `subtract()` может принимать любой итерируемый объект: список, строку, кортеж и т.д.

Приведенный ниже код:

```python
from collections import Counter

counter = Counter(i=4, s=40, a=1, p=20, b=98, z=69)
letters = 'iisssssapppz'

counter.subtract(letters)       # обновляем значения в counter

print(counter)
```

выводит:

```no-highlight
Counter({'b': 98, 'z': 68, 's': 35, 'p': 17, 'i': 2, 'a': 0})
```
