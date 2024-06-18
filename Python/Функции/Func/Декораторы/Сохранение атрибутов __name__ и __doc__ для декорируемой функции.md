

Как мы уже знаем, все функции содержат специальные [[Атрибут __name__]] и [[Атрибут __doc__]], которые содержат полезную информацию:

- `__name__` — имя функции
- `__doc__` — строка документации

Приведенный ниже код:

```python
def greet(name):
    '''Функция приветствия пользователя.'''
    return f'Hello {name}!'

print(greet.__name__)
print(greet.__doc__)
```

выводит:

```no-highlight
greet
Функция приветствия пользователя.
```

Рассмотрим применение декоратора `bold` к функции `greet()`.

Приведенный ниже код:

```python
def bold(func):
    def wrapper(*args, **kwargs):
        return '<b>' + func(*args, **kwargs) + '</b>'
    return wrapper

@bold
def greet(name):
    '''Функция приветствия пользователя.'''
    return f'Hello {name}!'

print(greet.__name__)
print(greet.__doc__)
```

выводит:

```no-highlight
wrapper
None
```

После того как к функции `greet()` был применен декоратор, её атрибуты `__name__` и `__doc__` изменились на имя и строку документации внутренней функции `wrapper()` декоратора `bold`. Хотя чисто технически это верно, это не очень хорошо.

Одно из решений такой проблемы выглядит следующим образом:

```python
def bold(func):
    def wrapper(*args, **kwargs):
        return '<b>' + func(*args, **kwargs) + '</b>'
    wrapper.__name__ = func.__name__
    wrapper.__doc__ = func.__doc__
    return wrapper

@bold
def greet(name):
    '''Функция приветствия пользователя.'''
    return f'Hello {name}!'

print(greet.__name__)
print(greet.__doc__)
```

Такой код выводит:

```no-highlight
greet
Функция приветствия пользователя.
```

Теперь у функции `greet()` атрибуты `__name__` и `__doc__` не перетираются после применения декоратора.