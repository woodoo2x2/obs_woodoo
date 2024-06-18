

В Python 3.10 появился метод `total()`, который вычисляет сумму всех значений `Counter` словаря, включая отрицательные.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(letters.total())
```

выводит:

```no-highlight
-87
```

В более ранних версиях Python приведенный выше код можно заменить кодом:

```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(sum(letters.values()))
```