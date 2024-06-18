
Для определения отсутствия общих элементов в множествах используется метод `isdisjoint()`. Данный метод возвращает значение `True`, если множества не имеют общих элементов, и  `False`, когда множества имеют общие элементы.

Приведенный ниже код:

```python
set1 = {1, 2, 3, 4, 5}
set2 = {5, 6, 7}
set3 = {7, 8, 9}

print(set1.isdisjoint(set2))
print(set1.isdisjoint(set3))
print(set2.isdisjoint(set3))
```

выводит:

```no-highlight
False
True
False
```
