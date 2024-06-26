Последний способ имитации нескольких инициализаторов заключается в создании универсальной функции одиночной диспетчеризации. Данный способ позволяет определить несколько инициализаторов и выборочно их использовать в зависимости от типа первого переданного в них аргумента.

**Универсальная функция** (generic function) представляет собой функцию, составленную из нескольких функций, реализующих одну и ту же операцию для различных типов. **Одиночная диспетчеризация** (single dispatch) — это алгоритм, который выбирает нужную реализацию на основе типа **одного аргумента**. Именно поэтому диспетчеризация называется одиночной.

Создание универсальной функции одиночной диспетчеризации происходит с помощью декоратора `@singledispatchmethod` из [[Модуль functools]], использование которого синтаксически схоже с использованием [[Декоратор @property]].

Приведенный ниже код:

```python
from functools import singledispatchmethod


class MyClass:
    @singledispatchmethod
    def base_implementation(self, arg):
        print('Базовая реализация')

    @base_implementation.register(int)
    def int_implementation(self, arg):
        print('Реализация для целочисленного аргумента')

    @base_implementation.register(str)
    def str_implementation(self, arg):
        print('Реализация для строкового аргумента')


obj = MyClass()

obj.base_implementation(1)
obj.base_implementation('bee')
obj.base_implementation([1, 2, 3])
```

определяет метод `base_implementation()`, имеющий две альтернативные реализации, и выводит:

```no-highlight
Реализация для целочисленного аргумента
Реализация для строкового аргумента
Базовая реализация
```

В примере выше мы сначала определяем метод `base_implementation()`, представляющий базовую реализацию, и применяем к нему декоратор `@singledispatchmethod`. Затем мы определяем метод `int_implementation()`, который является альтернативной реализацией метода `base_implementation()`, если в него будет передан целочисленный аргумент, и декорируем его с помощью `@base_implementation.register` с соответствующим аргументом `int`. Другими словами, декоратор `@base_implementation.register` закрепляет за методом `base_implementation()` метод `int_implementation()` и вызывает его, если передаваемый в него аргумент принадлежит типу `int`. Далее аналогично определяем альтернативную реализацию для строкового аргумента.

При вызове метода `base_implementation()` нужная реализация выбирается на основе типа аргумента, следующего после `self`. Если аргумент принадлежит типу `int`, будет вызван метод `int_implementation()`, если аргумент принадлежит типу `str` — метод `str_implementation()`, во всех остальных случаях — метод `base_implementation()`.
При использовании декоратора `@singledispatchmethod` альтернативные реализации не должны иметь то же имя, что и базовая реализация.

Используя данный способ для перегрузки метода [[Магический метод __init__()|__init__()]], мы можем сымитировать наличие в классе нескольких инициализаторов.

Приведенный ниже код:

```python
from functools import singledispatchmethod


class Cat:
    @singledispatchmethod
    def __init__(self, breed, name, age):
        self.breed = breed
        self.name = name
        self.age = age

    @__init__.register(list)
    def _from_list(self, data):
        self.breed, self.name, self.age = data


cat1 = Cat('Британский', 'Кемаль', 1)                         # передаем все данные отдельно
cat2 = Cat(['Манчкин', 'Роджер', 1])                          # передаем список с данными

print(cat1.breed, cat1.name, cat1.age)
print(cat2.breed, cat2.name, cat2.age)
```

выводит:

```no-highlight
Британский Кемаль 1
Манчкин Роджер 1
```

Имена методов, являющихся альтернативными, зачастую предваряют одним символом нижнего подчеркивания, тем самым делая их защищенными.