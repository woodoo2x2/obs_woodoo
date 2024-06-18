

Метод `most_common()` возвращает список наиболее повторяемых элементов и количество каждого из них. Метод возвращает список кортежей вида `(ключ, число повторений)`.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common())
print(numbers.most_common())
```

выводит:

```no-highlight
[('i', 4), ('s', 4), ('p', 2), ('m', 1)]
[(5, 3), (7, 3), (9, 3), (1, 2), (6, 1), (3, 1), (2, 1)]
```

Если методу `most_common()` передать целочисленный аргумент `n`, то он вернет `n` самых часто повторяющихся элементов.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common(3))
print(numbers.most_common(4))
```

выводит:

```no-highlight
[('i', 4), ('s', 4), ('p', 2)]
[(5, 3), (7, 3), (9, 3), (1, 2)]
```

Для поиска самых редких элементов, можно использовать срезы с отрицательным шагом.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common()[-1])
print(letters.most_common()[::-1])
print(numbers.most_common()[-3:-1])
```

выводит:

```no-highlight
('m', 1)
[('m', 1), ('p', 2), ('s', 4), ('i', 4)]
[(6, 1), (3, 1)]
```

)Для корректной работы метода `most_common()` нужно, чтобы значения в `Counter` словаре поддерживали сортировку.