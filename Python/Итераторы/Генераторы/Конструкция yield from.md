

Рассмотрим [[Функции Генераторы]] `get_data()`, которая порождает последовательность чисел `0, 1, 2, 3, 4`, а затем символов `A, B, C`:

```python
def get_data():
    for num in range(5):
        yield num
    for char in 'ABC':
        yield char
```

Приведенный ниже код:

```no-highlight
for i in get_data():
    print(i)
```

выводит:

```no-highlight
0
1
2
3
4
A
B
C
```

Генераторную функцию `get_data()` можно упростить, если использовать синтаксическую конструкцию `yield from <iterable>`, которая появилась в Python 3.3.

Приведенный ниже код равнозначен первому определению генераторной функции `get_data()`:

```python
def get_data():
    yield from range(5)
    yield from 'ABC'
```

Таким образом, синтаксическая конструкция `yield from <iterable>` позволяет объединить две конструкции: `yield` и цикл `for`.

Реализуем генераторную функцию `chain(*iterables)`, которая принимает произвольное количество итерируемых объектов и возвращает генератор, который последовательно порождает все значения сначала первого итерируемого объекта, затем второго, третьего и т.д.

Приведенный ниже код:

```python
def chain(*iterables):
    for it in iterables:
        for value in it:
            yield value

for i in chain('AB', [1, 2], (4, 5), {'name': 'Timur', 'age': 29}):
    print(i, end=' ')
```

выводит:

```no-highlight
A B 1 2 4 5 name age 
```

С помощью конструкции `yield from <iterable>` мы можем упростить тело генераторной функции`chain()`:

```python
def chain(*iterables):
    for it in iterables:
        yield from it
```

Как мы видим, конструкция `yield from` полностью заменяет внутренний цикл `for` и код действительно смотрится несколько проще.

Объединение конструкции `yield` и цикла `for` лишь часть возможностей `yield from`. На самом деле конструкция `yield from` позволяет вкладывать один генератор в другой, таким образом создавать субгенераторы (вложенные генераторы).

Приведенный ниже код:

```python
def generator2():
    yield 'Red'
    yield 'Blue'

def generator1():
    yield 'Green'
    yield from generator2()            # запрашиваем значение из субгенератора
    yield 'Yellow'
    yield 'Black'

for color in generator1():
    print(color, end=' ')
```

выводит:

```no-highlight
Green Red Blue Yellow Black 
```

Когда генератор `generator1()` вызывает `yield from generator2()`, субгенератор `generator2()` перехватывает управление и начинает отдавать значения туда, откуда был вызван `generator1()`. А тем временем `generator1()` остается блокированным в ожидании завершения `generator2()`. Таким образом, эффект получается таким же, как если бы тело субгенератора было встроено в месте, где находится выражение `yield from`.

Конструкция `yield from` позволяет программисту легко управлять сразу несколькими генераторами, настраивать их взаимодействие.