## Тип данных defaultdict

Основная проблема при работе с обычными словарями ([[Тип данных dict]]) – это попытка получить доступ (или изменить значение) по несуществующему ключу. Это приводит к возникновению ошибки `KeyError`. Несмотря на то что мы научились решать данную проблему штатными способами, стандартная библиотека предоставляет тип данных `defaultdict`.

Тип `defaultdict` ведет себя почти так же, как обычный словарь `dict`, но если мы попытаемся получить доступ (или изменить значение) по несуществующему ключу, то `defaultdict` автоматически создаст ключ и сгенерирует для него значение по умолчанию. Такое поведение делает `defaultdict` удобным вариантом обработки недостающих ключей в словарях.

В приведенном ниже коде мы обращаемся к несуществующему ключу в словаре `info`:

```python
info = {'name': 'Timur', 'age': 29, 'job': 'Teacher'}

print(info['salary'])
```

что приводит к возникновению ошибки:

```no-highlight
KeyError: 'salary'
```

Существует несколько способов с помощью которых можно избежать возникновения такой ошибки:

- [[Метод setdefault()(dict)]]
- [[Метод get()(dict)]]
- проверка наличия ключа с помощью оператора принадлежности (`key in dict`)

    

Следует отметить, что использование квадратных скобок (индексаторов) очень удобно и куда приятнее использования методов `setdefault()` и `get()`.

Для того чтобы не возникало ошибки при обращении по несуществующему ключу с помощью квадратных скобок, достаточно использовать альтернативный вариант словаря – `defaultdict`.

Приведенный ниже код:

```python
from collections import defaultdict

info = defaultdict(int)       # создаем словарь со значением по умолчанию 0

info['name'] = 'Timur'
info['age'] = 29
info['job'] = 'Teacher'

print(info['salary'])
print(info)
```

выводит:

```no-highlight
0
defaultdict(<class 'int'>, {'name': 'Timur', 'age': 29, 'job': 'Teacher', 'salary': 0})
```

Функция `defaultdict()` принимает в качестве аргумента тип элемента по умолчанию. Таким образом, для ключей, к которым происходит обращение, словарь `defaultdict` поставит в соответствие дефолтный элемент данного типа:

- для [[int (Целочисленный тип данных)]] – число `0`
- для [[float (Числа с плавающей точкой)]] – число `0.0`
- для [[bool (Логический тип данных)]] – значение `False`
- для [[str (Строки)]] – пустая строка `''`
- для [[list (Списки)]] – пустой список `[]`
- для [[Кортежи (tuple)]] – пустой кортеж `()`
- для [[set (Множества)]] – пустое множество `set()`
- для [[dict (Словарь)]] – пустой словарь `{}`

Помимо первого аргумента – типа элемента по умолчанию – мы можем передать второй аргумент: словарь, на основании которого будет создан `defaultdict`.

Приведенный ниже код:

```python
from collections import defaultdict

info = defaultdict(int, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})

print(info['name'])
print(info['salary'])
print(info)
```

выводит:

```no-highlight
Timur
0
defaultdict(<class 'int'>, {'name': 'Timur', 'age': 29, 'job': 'Teacher', 'salary': 0})
```

Также допустимы все способы, которые мы используем при создании обычных словарей, а именно передача именованных аргументов или итерируемого объекта, содержащего пары ключ-значение (например, список кортежей).

Приведенный ниже код:

```python
from collections import defaultdict

info1 = defaultdict(int, name='Timur', age=29, job='Teacher')
info2 = defaultdict(int, [('name', 'Timur'), ('age', 29), ('job', 'Teacher')])

print(info1)
print(info2)
```

выводит:

```no-highlight
defaultdict(<class 'int'>, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})
defaultdict(<class 'int'>, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})
```

Обратите внимание, что при создании словаря `defaultdict` мы можем указать только именованные аргументы, но не можем указать только итерируемый объект с парами ключ-значение (или словарь).

Приведенный ниже код:

```python
from collections import defaultdict

info = defaultdict(name='Timur', age=29, job='Teacher')

print(info)
```

выводит: 

```no-highlight
defaultdict(None, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})
```

В то время как следующий код:

```python
from collections import defaultdict

info = defaultdict([('name', 'Timur'), ('age', 29), ('job', 'Teacher')])

print(info)
```

приводит к возникновению ошибки, так как в качестве первого аргумента должен быть указан тип элемента по умолчанию, а не итерируемый объект с парами ключ-значение (или словарь).

**Рассмотрим задачу:** пусть задан список чисел `numbers`, в котором некоторые числа встречаются несколько раз. Нужно узнать, сколько именно раз встречается каждое из чисел.

**Решение 1.** С помощью метода `setdefault()`:

```python
numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]

result = {}
for num in numbers:
    value = result.setdefault(num, 0)
    result[num] = value + 1
```

**Решение 2.** С помощью метода `get()`:

```python
numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]

result = {}
for num in numbers:
    result[num] = result.get(num, 0) + 1
```

**Решение 3.** С помощью оператора принадлежности `in`:

```python
numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]

result = {}
for num in numbers:
    if num not in result:
        result[num] = 1
    else:
        result[num] += 1
```

**Решение 4.** С помощью типа данных `defaultdict`:

```python
from collections import defaultdict

numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]
result = defaultdict(int)

for num in numbers:
    result[num] += 1
```

Тип данных `defaultdict` часто используют в связке с пустым списком в качестве значения по умолчанию, чтобы начинать добавление элементов без лишнего кода.

Приведенный ниже код работает так, как полагается:

```python
from collections import defaultdict

my_dict = defaultdict(list)

for i in range(7):
    my_dict[i].append(i)

for key in my_dict:
    print(key, my_dict[key])
```

и выводит:

```no-highlight
0 [0]
1 [1]
2 [2]
3 [3]
4 [4]
5 [5]
6 [6]
```

При использовании `defaultdict` нет необходимости ни проверять наличие соответствующих ключей в словаре, ни создавать предварительно пустые списки.

## Когда использовать defaultdict?

Приведем несколько рекомендаций, когда удобно использовать `defaultdict`, вместо `dict`:

1. Если ваш код в значительной степени основан на словарях и вы все время имеете дело с отсутствующими ключами, вам следует подумать об использовании `defaultdict`, а не обычного `dict`
2. Если элементы вашего словаря необходимо инициализировать некоторым значением по умолчанию, вам следует подумать об использовании `defaultdict`, вместо `dict`
3. Если ваш код использует словари для агрегирования, накопления, подсчета или группировки значений, вам следует подумать об использовании `defaultdict`, вместо `dict`

## Примечания

**Примечание 1.** Тип `defaultdict` наследуется от типа `dict`.

Приведенный ниже код:

```python
from collections import defaultdict

print(issubclass(defaultdict, dict))
```

выводит:

```no-highlight
True
```

Таким образом, все методы доступные для обычных словарей (тип `dict`), также доступны и для `defaultdict` словарей.



Мы можем сравнивать обычные словари (тип `dict`) и `defaultdict` словари.

Приведенный ниже код:

```python
from collections import defaultdict

info1 = {'name': 'Timur', 'age': 29, 'job': 'Teacher'}
info2 = defaultdict(int, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})

print(info1 == info2)
```

выводит:

```no-highlight
True
```

При создании `defaultdict` словаря можно указывать не только тип данных для значений по умолчанию, но и любую функцию, **не принимающую аргументов** и **возвращающую некоторое дефолтное значение**.

Приведенный ниже код (передаем функцию, объявленную с помощью `def`):

```python
from collections import defaultdict

def get_default():
    return 69

info = defaultdict(get_default, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})

print(info['name'])
print(info['salary'])
```

выводит:

```no-highlight
Timur
69
```

 Приведенный ниже код (передаем функцию, объявленную с помощью `lambda`):

```python
from collections import defaultdict

info = defaultdict(lambda: '1000000$', {'name': 'Timur', 'age': 29, 'job': 'Teacher'})

print(info['name'])
print(info['salary'])
```

выводит:

```no-highlight
Timur
1000000$
```

Обратите внимание, что передаваемая функция не должна принимать никаких аргументов.

Приведенный ниже код:

```python
from collections import defaultdict

def get_default(x):
    return 2 * x

info = defaultdict(get_default, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})

print(info['name'])
print(info['salary'])
```

приводит к возникновению ошибки.

 Если создать экземпляр `defaultdict` словаря без указания `default_factory` (значения по умолчанию для отсутствующих ключей), то поведение `defaultdict` будет таким же, как и у обычного словаря (тип `dict`).

Приведенный ниже код:

```python
from collections import defaultdict

data = defaultdict()

print(data['salary'])
```

приводит к возникновению ошибки `KeyError`.

Аналогичное поведение будет, если в качестве `default_factory` передать значение [[None (NoneType)]].

Приведенный ниже код:

```python
from collections import defaultdict

data = defaultdict(None)

print(data['salary'])
```

также приводит к возникновению ошибки `KeyError`.

  Значение `None` является значением по умолчанию для `default_factory`.

 Функцию, которая возвращает значение по умолчанию для отсутствующих ключей, можно явно менять через атрибут `default_factory`.

Приведенный ниже код:

```python
from collections import defaultdict

data = defaultdict(int)
print(data['salary1'])

data.default_factory = list
print(data['salary2'])

data.default_factory = float
print(data['salary3'])
```

выводит:

```no-highlight
0
[]
0.0
```

Тип `defaultdict` работает быстрее чем использование методов `setdefault()` и `get()` обычного словаря (тип `dict`).

Прекрасная статья по типу `defaultdict` доступна по [ссылке](https://realpython.com/python-defaultdict/).

 Исходный код `defaultdict` доступен по [ссылке](https://github.com/python/cpython/blob/main/Modules/_collectionsmodule.c) и [ссылке](https://gist.github.com/ohe/1605376). Рекомендуем вам ознакомиться с ним, тогда вопросов точно не останется 😎.