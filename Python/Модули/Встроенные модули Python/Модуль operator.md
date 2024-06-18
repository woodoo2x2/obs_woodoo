

Чтобы не писать каждый раз функции, определяющие такие стандартные математические операции как сумма или произведение:

```python
def sum_func(a, b):
    return a + b


def mult_func(a, b):
    return a * b
```

можно использовать уже реализованные функции из модуля `operator`.

Неполный список функций из модуля `operator` выглядит так:

|   |   |   |
|---|---|---|
|**Операция**|**Синтаксис**|**Функция**|
|Addition|`a + b`|`add(a, b)`|
|Containment Test|`obj in seq`|`contains(seq, obj)`|
|Division|`a / b`|`truediv(a, b)`|
|Division|`a // b`|`floordiv(a, b)`|
|Exponentiation|`a ** b`|`pow(a, b)`|
|Modulo|`a % b`|`mod(a, b)`|
|Multiplication|`a * b`|`mul(a, b)`|
|Negation (Arithmetic)|`-a`|`neg(a)`|
|Subtraction|`a - b`|`sub(a, b)`|
|Ordering|`a < b`|`lt(a, b)`|
|Ordering|`a <= b`|`le(a, b)`|
|Equality|`a == b`|`eq(a, b)`|
|Difference|`a != b`|`ne(a, b)`|
|Ordering|`a >= b`|`ge(a, b)`|
|Ordering|`a > b`|`gt(a, b)`|

Рассмотрим примеры использования функций из модуля `operator`.

Приведенный ниже код:

```python
from operator import *     #  импортируем все функции

print(add(10, 20))         #  сумма
print(floordiv(20, 3))     #  целочисленное деление
print(neg(9))              #  смена знака
print(lt(2, 3))            #  проверка на неравенство <
print(lt(10, 8))           #  проверка на неравенство <
print(eq(5, 5))            #  проверка на равенство ==
print(eq(5, 9))            #  проверка на равенство ==
```

выводит:

```no-highlight
30
6
-9
True
False
True
False
```

Приведенный ниже код:

```python
from functools import reduce
import operator

words = ['Testing ', 'shows ', 'the ', 'presence', ', ', 'not ', 'the ', 'absence ', 'of ', 'bugs'] 
numbers = [1, 2, -6, -4, 3, 9, 0, -6, -1]

opposite_numbers = list(map(operator.neg, numbers))    #  смена знаков элементов списка
concat_words = reduce(operator.add, words)             #  конкатенация элементов списка

print(opposite_numbers)
print(concat_words)
```

Выводит:

```no-highlight
[-1, -2, 6, 4, -3, -9, 0, 6, 1]
Testing shows the presence, not the absence of bugs
```

Модуль `operator` реализован на языке `C`, поэтому функции этого модуля работают в разы быстрее, чем самописные функции в Python.