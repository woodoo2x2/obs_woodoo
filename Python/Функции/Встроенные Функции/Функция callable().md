

Функция `callable()` принимает в качестве аргумента некоторый объект и возвращает `True`, если переданный объект является вызываемым, или `False` в противном случае.

Приведенный ниже код:

```python
print(callable(int))
print(callable(list))
print(callable(100))
print(callable([1, 2, 3]))
```

выводит:

```no-highlight
True
True
False
False
```
