

Замороженное множество (`frozenset`) также является встроенной коллекцией в Python. Обладая характеристиками обычного множества, замороженное множество не может быть изменено после создания.

Кортеж (тип `tuple`) – неизменяемая версия списка (тип `list`), а замороженное множество (тип `frozenset`) – неизменяемая версия обычного множества (тип `set`).

Для создания замороженного множества используется встроенная функция `frozenset()`, которая принимает в качестве аргумента другую коллекцию.

Приведенный ниже код:

```python
myset1 = frozenset({1, 2, 3})                         # на основе множества
myset2 = frozenset([1, 1, 2, 3, 4, 4, 4, 5, 6, 6])    # на основе списка
myset3 = frozenset('aabcccddee')                      # на основе строки

print(myset1)
print(myset2)
print(myset3)
```

выводит:

```no-highlight
frozenset({1, 2, 3})
frozenset({1, 2, 3, 4, 5, 6})
frozenset({'e', 'd', 'c', 'b', 'a'})
```

Будучи изменяемыми, обычные множества не могут быть элементами других множеств. Замороженные множества являются неизменяемыми, а значит могут быть элементами других множеств.

Приведенный ниже код:

```python
sentence = 'The cat in the hat had two sidekicks, thing one and thing two.'

words = sentence.lower().replace('.', '').replace(',', '').split()

vowels = ['a', 'e', 'i', 'o', 'u']

consonants = {frozenset({letter for letter in word if letter not in vowels}) for word in words}

print(*consonants, sep='\n')
```

выводит (порядок элементов может отличаться):

```no-highlight
frozenset({'d', 'h'})
frozenset({'h', 't'})
frozenset({'n', 'h', 'g', 't'})
frozenset({'n'})
frozenset({'c', 't'})
frozenset({'n', 'd'})
frozenset({'w', 't'})
frozenset({'s', 'c', 'k', 'd'})
```