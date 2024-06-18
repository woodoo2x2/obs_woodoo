
Метод `elements()` возвращает итератор по элементам, в котором каждый элемент повторяется столько раз, во сколько установлено его значение. Элементы возвращаются в порядке их появления.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(list(letters.elements()))
print(list(numbers.elements()))
```

выводит:

```no-highlight
['m', 'i', 'i', 'i', 'i', 's', 's', 's', 's', 'p', 'p']
[5, 5, 5, 6, 7, 7, 7, 1, 1, 3, 9, 9, 9, 2]
```

Метод `elements()` возвращает итератор. Для того чтобы увидеть его элементы, требуется преобразовать его в список с помощью [[Функция list()]].

Если количество элементов по некоторому ключу меньше единицы, то метод `elements()` просто проигнорирует его.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(list(letters.elements()))
```

выводит:

```no-highlight
['i', 'i', 'i', 'i', 's', 's', 's', 's', 'p', 'p', 'm']
```