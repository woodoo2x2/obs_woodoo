

Доступ к элементам и итерирование по `OrderedDict` словарям работает так же, как и у обычных словарей. Мы можем перебирать ключи напрямую или можем использовать словарные методы [[Метод items()(dict)]], [[Метод keys()(dict)]] и [[Метод values()(dict)]].

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

# обращение по ключу
print(numbers['one'])
print(numbers['three'])

print()

# перебор ключей напрямую
for key in numbers:
    print(key, '->', numbers[key])

print()

# перебор пар (ключ, значение) через метод
for key, value in numbers.items():
    print(key, '->', value)

print()

# перебор ключей через метод
for key in numbers.keys():
    print(key, '->', numbers[key])

print()

# перебор значений через метод
for value in numbers.values():
    print(value)
```

выводит:

```no-highlight
1
3

one -> 1
two -> 2
three -> 3

one -> 1
two -> 2
three -> 3

one -> 1
two -> 2
three -> 3

1
2
3
```

При итерировании по `OrderedDict` словарям мы можем использовать встроенную функцию [[Функция reversed()]].

Приведенный ниже код:

```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

# перебор ключей напрямую
for key in reversed(numbers):
    print(key, '->', numbers[key])

print()

# перебор пар (ключ, значение) через метод
for key, value in reversed(numbers.items()):
    print(key, '->', value)

print()

# перебор ключей через метод
for key in reversed(numbers.keys()):
    print(key, '->', numbers[key])

print()

# перебор значений через метод
for value in reversed(numbers.values()):
    print(value)
```

выводит:

```no-highlight
three -> 3
two -> 2
one -> 1

three -> 3
two -> 2
one -> 1

three -> 3
two -> 2
one -> 1

3
2
1
```

 Обычные словари [[Тип данных dict]] начиная с Python 3.8 также поддерживают использование встроенной функции `reversed()`.