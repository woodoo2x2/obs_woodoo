

Функция `suppress()` используется для построения контекстных менеджеров, которые игнорируют заданные типы исключений.

Примерный код функции `suppress()`:

```python
from contextlib import contextmanager

@contextmanager
def suppress(*exceptions):
    try:
        yield
    except Exception as error:
        if type(error) not in exceptions:
            raise
```

Приведенный ниже код:

```python
from contextlib import suppress

with suppress(ValueError):
    num = int('python')

print('beegeek')

with suppress(TypeError, ZeroDivisionError):
    num = 1 / 0

print('pygen')
```

выводит:

```no-highlight
beegeek
pygen
```

Как и с любым другим механизмом, который полностью подавляет исключения, этот контекстный менеджер следует использовать только для покрытия специфических ошибок, когда точно известно, что продолжение работы программы без вывода сообщений будет корректным.

 Контекстный менеджер, возвращаемый функцией `suppress()`, является [[Реентерабельные контекстные менеджеры|реентерабельным]].