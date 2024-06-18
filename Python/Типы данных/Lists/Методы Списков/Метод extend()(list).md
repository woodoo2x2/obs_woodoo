
Можно также расширить список другим списком путем вызова метода `extend()`.

Следующий программный код:

```python
numbers = [0, 2, 4, 6, 8, 10]
odds = [1, 3, 5, 7]

numbers.extend(odds)
print(numbers)
```

выведет:

```no-highlight
[0, 2, 4, 6, 8, 10, 1, 3, 5, 7]
```

Метод `extend()` как бы расширяет один список, добавляя к нему элементы другого списка.

Отличие между методами `append()` и `extend()` проявляется при добавлении строки к списку.

Следующий программный код:

```python
words1 = ['iq option', 'stepik', 'beegeek']
words2 = ['iq option', 'stepik', 'beegeek']

words1.append('python')
words2.extend('python')

print(words1)
print(words2)
```

выведет:

```no-highlight
['iq option', 'stepik', 'beegeek', 'python']
['iq option', 'stepik', 'beegeek', 'p', 'y', 't', 'h', 'o', 'n']
```

Метод `append()` добавляет строку `'python'` целиком к списку, а метод `extend()` разбивает строку `'python'` на  символы `'p'`, `'y'`, `'t'`, `'h'`, `'o'`, `'n'` и их добавляет в качестве элементов списка. 