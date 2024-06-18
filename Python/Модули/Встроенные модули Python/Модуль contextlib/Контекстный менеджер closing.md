Функция `closing()` используется для построения контекстных менеджеров из объектов, которые предоставляют метод `close()`, но не реализуют протокол контекстного менеджера (отсутствуют методы `__enter__()` и `__exit__()`).

Примерный код функции `closing()`:

```python
from contextlib import contextmanager

@contextmanager
def closing(thing):
    try:
        yield thing
    finally:
        thing.close()
```

Приведенный ниже код:

```python
from contextlib import closing

class Cat:
    def __init__(self, name):
        self.name = name
        
    def __str__(self):
        return self.name

    def close(self):
        print('Пока,', self.name)
    

with closing(Cat('Кемаль')) as cat:
    print('Привет,', cat)
```

выводит:

```no-highlight
Привет, Кемаль
Пока, Кемаль
```

Метод `close()` объекта `cat` будет вызываться всегда, даже если в блоке `with` возникает ошибка.

  В случае отсутствия метода `close()` у объекта возбуждается исключение `AttributeError`.