Реализации собственные контекстные менеджеры путем создания классов, содержащих два магических метода [[Магический метод __enter__()]] и [[Магический метод __exit__()]] для поддержания протокола контекстного менеджера. Делать это было несложно, однако достаточно долго и не всегда удобно.

В Python создавать контекстные менеджеры можно намного проще с помощью декоратора `@contextmanager` из [[Модуль context lib|модуля contextlib]]. Декоратор `@contextmanager` позволяет создать контекстный менеджер на основе функции, автоматически предоставляя оба требуемых метода `__enter__()` и `__exit__()`.

Контекстный менеджер `CustomContextManager`, который просто выводит текст в методах `__enter__()` и `__exit__()`:

```python
class CustomContextManager:
    def __enter__(self):
        print('Вход в контекстный менеджер...')
        return 'Python generation!'

    def __exit__(self, exc_type, exc_value, traceback):
        print('Выход из контекстного менеджера...')
```

с использованием декоратора `@contextmanager` можно переписать в виде:

```python
from contextlib import contextmanager

@contextmanager
def custom_context_manager():
    print('Вход в контекстный менеджер...')
    yield 'Python generation!'
    print('Выход из контекстного менеджера...')
```

Приведенный ниже код:

```python
with custom_context_manager() as manager:
    print(manager)
```

выводит:

```no-highlight
Вход в контекстный менеджер...
Python generation!
Выход из контекстного менеджера...
```

Как мы видим, код до оператора `yield` выполняется, когда поток выполнения входит в контекст и соответствует методу `__enter__()`. Значение, которое возвращает оператор `yield` является возвращаемым значением метода `__enter__()`. Наконец, код после оператора `yield` выполняется, когда поток выполнения выходит из контекста, что соответствует методу `__exit__()`.

## Обработка исключений внутри блока with

Важным моментом при написании контекстных менеджеров является обработка исключений, которые возбуждаются внутри блока [[Оператор with|with]]. Для обработки исключений при использовании контекстных менеджеров на основе классов, мы использовали метод `__exit__()`, который возвращает логический флаг ([[Тип данных bool]]), указывающий на то, следует ли подавить возбужденное исключение.

Для обработки исключений в контекстных менеджерах на основе функций мы должны использовать конструкцию `try-except-finally`.

Контекстный менеджер `CustomContextManager`, который просто выводит текст в методах `__enter__()` и `__exit__()` и обрабатывает исключения типа `IndexError`:

```python
class CustomContextManager:
    def __enter__(self):
        print('Вход в контекстный менеджер...')
        return 'Python generation!'

    def __exit__(self, exc_type, exc_value, traceback):
        print('Выход из контекстного менеджера...')
        if isinstance(exc_value, IndexError):
            print(f'Тип возникшего исключения: {exc_type}')
            print(f'Текст исключения: {exc_value}')
            return True                                 # подавляем возбужденное исключение IndexError
        return False
```

с использованием декоратора `@contextmanager` можно переписать в виде:

```python
from contextlib import contextmanager

@contextmanager
def custom_context_manager():
    print('Вход в контекстный менеджер...')
    try:
        yield 'Python generation!'
    except IndexError as error:
        print(f'Тип возбужденного исключения: {type(error)}')
        print(f'Текст исключения: {error}')
    except:
        raise           # если исключение не планируется подавлять, оно должно быть возбуждено повторно
    finally:
        print('Выход из контекстного менеджера...')
```

Приведенный ниже код:

```python
with custom_context_manager() as manager:
    print(manager)
    print(manager[100])                                 # обращаемся по несуществующему индексу
```

выводит:

```no-highlight
Вход в контекстный менеджер...
Python generation!
Тип возбужденного исключения: <class 'IndexError'>
Текст исключения: string index out of range
Выход из контекстного менеджера...
```

Как мы видим, для обработки определенного типа исключения внутри блока `with` нужно добавить соответствующий блок `except <тип исключения>`, в котором нужным образом обработать возбужденное исключение, что соответствует инструкции `return True` метода `__exit__()`. В случае если возникшее исключение не требуется обрабатывать, оно должно быть возбуждено заново с помощью оператора `raise`, что соответствует инструкции `return False` метода `__exit__()`.

 

## Примеры создания контекстных менеджеров

Перепишем созданные в прошлом уроке контекстные менеджеры с помощью декоратора `@contextmanager`.

**Пример 1.** Контекстный менеджер `Trace` выводит информацию перед входом в блок `with` и после выхода из блока `with`, включая информацию об исключении, если оно было возбуждено во время выполнения блока `with`.

```python
class Trace:
    def __enter__(self):
        print('Начало выполнения блока with')

    def __exit__(self, exc_type, exc_value, traceback):
        if exc_value:
            print(f'Во время выполнения блока with возникло исключение {exc_value}')
        print('Конец выполнения блока with')
        return True                           # обрабатываем все типы исключений
```

С помощью декоратора `@contextmanager` контекстный менеджер `Trace` выглядит так:

```python
from contextlib import contextmanager

@contextmanager
def trace():
    print('Начало выполнения блока with')
    try:
        yield
    except Exception as error:
        print(f'Во время выполнения блока with возникло исключение {error}')
    finally:
        print('Конец выполнения блока with')
```

Приведенный ниже код:

```python
with trace():
    print('Python generation!')

print()

with trace():
    print('Python generation!')
    print(1 / 0)
```

выводит:

```no-highlight
Начало выполнения блока with
Python generation!
Конец выполнения блока with

Начало выполнения блока with
Python generation!
Во время выполнения блока with возникло исключение division by zero
Конец выполнения блока with
```

**Пример 2.** Контекстный менеджер `WritableTextFile` позволяет работать с открытыми для записи текстовыми файлами в кодировке `UTF-8`:

```python
class WritableTextFile:
    def __init__(self, path):
        self.path = path

    def __enter__(self):
        self.file = open(self.path, mode='w', encoding='utf-8')
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        self.file.close()
```

С помощью декоратора `@contextmanager` контекстный менеджер `WritableTextFile` выглядит так:

```python
from contextlib import contextmanager

@contextmanager
def writable_text_file(path):
    file = open(path, mode='w', encoding='utf-8')
    yield file
    file.close()
```

Приведенный ниже код:

```python
with writable_text_file('output.txt') as file:
    file.write('Python generation!')
```

создает текстовый файл `output.txt` в кодировке `UTF-8` с содержимым `Python generation!`.

**Пример 3.** Контекстный менеджер `RedirectedStdout` **временно** перенаправляет стандартный вывод `sys.stdout` на некоторый файл на диске:

```python
import sys

class RedirectedStdout:
    def __init__(self, new_output):
        self.new_output = new_output

    def __enter__(self):
        self.standart_output = sys.stdout
        sys.stdout = self.new_output

    def __exit__(self, exc_type, exc_value, traceback):
        sys.stdout = self.standart_output 
```

С помощью декоратора `@contextmanager` контекстный менеджер `RedirectedStdout` выглядит так:

```python
import sys
from contextlib import contextmanager

@contextmanager
def redirected_stdout(new_output):
    standart_output = sys.stdout
    sys.stdout = new_output
    yield
    sys.stdout = standart_output
```

Приведенный ниже код:

```python
with open('output.txt', mode='w', encoding='utf-8') as file:
    with redirected_stdout(file):
        print('Python generation!')
    print('Возврат к стандартному потоку вывода')
```

создает текстовый файл `output.txt` в кодировке `UTF-8` с содержимым `Python generation!`, а также выводит текст `Возврат к стандартному потоку вывода`.

**Пример 4.** Контекстный менеджер `Timer` позволяет измерять время выполнения блока кода:

```python
from time import perf_counter

class Timer:
    def __enter__(self):
        self.start = perf_counter()
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        self.elapsed = perf_counter() - self.start
```

С помощью декоратора `@contextmanager` контекстный менеджер `Timer` выглядит так:

```python
from time import perf_counter
from contextlib import contextmanager

@contextmanager
def timer():
    start = perf_counter()
    yield
    end = perf_counter()
    elapsed = end - start
    print('Затраченное время:', elapsed)
```

Приведенный ниже код:

```python
from time import sleep

with timer():
    sleep(0.7)
    sleep(1.5)
```

выводит (время может отличаться):

```no-highlight
Затраченное время: 2.208050799788907
```

Декоратор `@contextmanager` – элегантный и практически полезный инструмент, который сводит воедино три совершенно разных механизма Python: декоратор функции, генератор и оператор `with`.

 Обычно с помощью декоратора `@contextmanager` создают **относительно несложные** контекстные менеджеры.

 Функция, к которой применяется декоратор `@contextmanager`, обязательно должна иметь инструкцию `yield`, в противном случае при попытке воспользоваться реализованной функцией как контекстным менеджером будет возбуждено исключение.

Приведенный ниже код:

```python
from contextlib import contextmanager

@contextmanager
def greeter(name):
    print('Привет,', name)
    return name
    print('Пока,', name)
    
with greeter('Гвидо') as manager:
    print(manager)
```

выводит:

```no-highlight
Привет, Гвидо
```

а затем приводит к возбуждению исключения:

```no-highlight
TypeError: 'str' object is not an iterator
```

Аналогично приведенный ниже код:

```python
from contextlib import contextmanager

@contextmanager
def greeter(name):
    print('Привет,', name)
    print('Пока,', name)
    
with greeter('Гвидо') as manager:
    print(manager)
```

выводит:

```no-highlight
Привет, Гвидо
Пока, Гвидо
```

а затем приводит к возбуждению исключения:

```no-highlight
TypeError: 'NoneType' object is not an iterator
```

Контекстные менеджеры созданные с помощью декоратора `@contextmanager` являются **одноразовыми**, их нужно создавать заново каждый раз, когда мы хотим их использовать. Попытка повторного использования такого контекстного менеджера приводит к возбуждению исключения `AttributeError`.

**Примечание 5.** Процесс создания контекстных менеджеров на основе функций похож на процесс создания итераторов с помощью генераторных функций. Подробнее о генераторных функциях можно почитать по [[Генератор|Функции генераторы]]

 При использовании контекстных менеджеров на основе классов мы могли достаточно легко передавать значения из метода `__enter__()` в тело блока `with`. Для этого нужно было использовать атрибуты объекта, возвращаемого методом `__enter__()`. При использовании контекстных менеджеров на основе функций мы можем добиться аналогичного поведения, если будем возвращать [[lambda-функции (Анонимные функции)]] 

Приведенный ниже код:

```python
from time import perf_counter, sleep
from contextlib import contextmanager

@contextmanager
def timer():
    start = perf_counter()
    end = 0
    yield lambda : (start, end)
    end = perf_counter()

with timer() as manager:
    sleep(1.4)
    print(manager())            # вызов внутри блока with

print(manager())                # вызов после блока with
start, end = manager()
print('Затраченное время:', end - start)
```

выводит (время может отличаться):

```no-highlight
(2127754.9660813, 0)
(2127754.9660813, 2127756.3803388)
Затраченное время: 1.414257500320673
```

 Внутри одного блока `with` могут использоваться несколько контекстных менеджеров.

Приведенный ниже код:

```python
from contextlib import contextmanager

@contextmanager
def greeter(name):
    print('Привет,', name)
    yield name
    print('Пока,', name)
    
with greeter('Илон') as manager1, greeter('Гвидо') as manager2, greeter('Хидео') as manager3:
    print(manager1, manager2, manager3)
```

выводит:

```no-highlight
Привет, Илон
Привет, Гвидо
Привет, Хидео
Илон Гвидо Хидео
Пока, Хидео
Пока, Гвидо
Пока, Илон
```

Обратите внимание, на порядок, в котором закрываются контекстные менеджеры: сперва закрывается третий, затем второй, и только потом первый. Если внутри блока `with` будет возбуждено исключение, этот порядок может быть важен, поскольку любой контекстный менеджер может подавить возбужденное исключение, и тогда остальные менеджеры даже не получат информацию об этом.