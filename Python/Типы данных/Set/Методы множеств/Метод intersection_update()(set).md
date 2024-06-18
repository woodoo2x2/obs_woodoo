

Метод `intersection_update()` изменяет исходное множество **по пересечению**.

Приведенный ниже код:

```python
myset1 = {1, 2, 3, 4, 5}
myset2 = {3, 4, 6, 7, 8}

myset1.intersection_update(myset2)  # изменяем множество myset1
print(myset1)
```

выводит (порядок элементов может отличаться):

```no-highlight
{3, 4}
```

Аналогичный результат получается, если использовать оператор `&=`:

```python
myset1 = {1, 2, 3, 4, 5}
myset2 = {3, 4, 6, 7, 8}

myset1 &= myset2
print(myset1)
```
