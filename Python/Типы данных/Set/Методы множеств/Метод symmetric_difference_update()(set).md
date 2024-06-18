

Метод `symmetric_difference_update()` изменяет исходное множество **по симметрической разности**.

Приведенный ниже код:

```python
myset1 = {1, 2, 3, 4, 5}
myset2 = {3, 4, 6, 7, 8}

myset1.symmetric_difference_update(myset2)  # изменяем множество myset1
print(myset1)
```

выводит (порядок элементов может отличаться):

```no-highlight
{1, 2, 5, 6, 7, 8}
```

Аналогичный результат получается, если использовать оператор `^=`:

```python
myset1 = {1, 2, 3, 4, 5}
myset2 = {3, 4, 6, 7, 8}

myset1 ^= myset2
print(myset1)
```