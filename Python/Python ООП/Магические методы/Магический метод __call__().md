Рассмотрим класс `Cat`, описывающий кошку. Определим в классе магический метод `__call__()`, при вызове которого будет выводиться сообщение с указанием имени кошки.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self.name = name                   # имя кошки

    def __call__(self):
        print('Меня зовут', self.name)


cat = Cat('Кемаль')

cat()                                      # равнозначно cat.__call__()
```

выводит:

```no-highlight
Меня зовут Кемаль
```

Магический метод `__call__()`, являясь обычным методом экземпляра, может принимать произвольное количество аргументов. И если они есть, то при вызове экземпляров класса их потребуется передать.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self.name = name

    def __call__(self, speech):
        print('Меня зовут', self.name, 'и я делаю', speech)


cat = Cat('Кемаль')

cat('Мяу')                                 # равнозначно cat.__call__('Мяу')
cat('Мяяяяяy')                             # равнозначно cat.__call__('Мяяяяяy')
```

выводит:

```python
Меня зовут Кемаль и я делаю Мяу
Меня зовут Кемаль и я делаю Мяяяяяy
```

## Сценарии использования метода __call__()

**Сценарий 1.** Класс с реализованным методом `__call__()` может быть хорошей альтернативой замыканиям.

Рассмотрим функцию `line_generator()`, представляющую генератор линейных функцийy=kx+b. Данная функция принимает в качестве аргументов параметры k и b и возвращает линейную функцию с данными параметрами.

Приведенный ниже код:

```python
def line_generator(k, b):
    def func(x):
        return k * x + b
    return func

line_func1 = line_generator(2, 5)          # получаем функцию y = 2*x + 5
line_func2 = line_generator(-6, 9)         # получаем функцию y = -6*x + 9

print(line_func1(10))                      # выводим значение 2*10 + 5 = 25
print(line_func2(4))                       # выводим значение -6*4 + 9 = -15
```

выводит:

```no-highlight
25
-15
```

Аналогичный генератор мы можем реализовать с помощью класса `LineGenerator`, определив в нем магический метод `__call__()`.

Приведенный ниже код:

```python
class LineGenerator:
    def __init__(self, k, b):
        self.k = k
        self.b = b

    def __call__(self, x):
        return self.k * x + self.b


line_func1 = LineGenerator(2, 5)           # получаем функцию y = 2*x + 5
line_func2 = LineGenerator(-6, 9)          # получаем функцию y = -6*x + 9

print(line_func1(10))                      # выводим значение 2*10 + 5 = 25
print(line_func2(4))                       # выводим значение -6*4 + 9 = -15
```

выводит:

```no-highlight
25
-15
```

Несмотря на то что объекты класса `LineGenerator` не являются функциями, они являются вызываемыми объектами, которые в полной мере выполняют поставленную задачу.

**Сценарий 2.** Метод `__call__()` позволяет реализовывать [[Декораторы]] на основе классов.

Рассмотрим декоратор `uppercase_decorator()`, который преобразовывает строковый результат декорируемой функции в верхний регистр.

Приведенный ниже код:

```python
def uppercase_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs).upper()
    return wrapper

@uppercase_decorator
def greet(name):
    return f'Привет, {name}'

print(greet('Кемаль'))
```

выводит:

```no-highlight
ПРИВЕТ, КЕМАЛЬ
```

Аналогичный декоратор мы можем реализовать с помощью класса `UppercaseDecorator`, определив в нем магический метод `__call__()`.

Приведенный ниже код:

```python
class UppercaseDecorator:
    def __init__(self, func):
        self.func = func
 
    def __call__(self, *args, **kwargs):
        return self.func(*args, **kwargs).upper()


@UppercaseDecorator
def greet(name):
    return f'Привет, {name}'

print(greet('Кемаль'))
```

выводит:

```no-highlight
ПРИВЕТ, КЕМАЛЬ
```

Как мы видим, принцип создания декораторов на основе классов довольно прост. Достаточно запомнить декорируемую функцию, а затем расширить ее функционал в магическом методе `__call__()`.

**Сценарий 3.** Магический метод `__call__()` может быть полезен в классах, чьи экземпляры часто изменяют своё состояние. Вызов экземпляра класса может быть интуитивно понятным способом изменить состояние объекта.

Рассмотрим класс `Point`, описывающий точку на плоскости. Определим в классе метод `__call__()`, позволяющий изменять координаты точки по обеим осям.

Приведенный ниже код:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __call__(self, x, y):
        self.x, self.y = x, y


point = Point(1, 2)
print(point.x, point.y)

point(3, 4)
print(point.x, point.y)
```

выводит:

```no-highlight
1 2
3 4
```

Реализация кеша:
```py
class CachedFunction:
    def __init__(self,func):
        self.func = func
        self.cache = {}
        
    def __call__(self,*args):
        if tuple(args) in self.cache:
            return self.cache[tuple(args)]
        else:
            self.cache[tuple(args)] = self.func(*args)
        return self.func(*args)
@CachedFunction
def slow_fibonacci(n):
    if n == 1:
        return 0
    elif n in (2, 3):
        return 1
    return slow_fibonacci(n - 1) + slow_fibonacci(n - 2)
    
slow_fibonacci(5)

for args, value in sorted(slow_fibonacci.cache.items()):
    print(args, value)
```