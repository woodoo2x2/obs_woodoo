

В модуле [[Модуль functools|`functools`]] уже реализован декоратор `lru_cache`, дающий возможность кэшировать результат вычисления функции, используя стратегию Least Recently Used. Это простой, но мощный метод, который позволяет использовать в коде возможности кэширования.

Декоратор `lru_cache` доступен начиная с Python 3.2.

Приведенный ниже код:

```python
from functools import lru_cache​

@lru_cache()
def fibonacci(n):
    if n <= 2:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```

добавляет кэширование к функции `fibonacci()`. Декоратор `lru_cache` сохраняет результаты вызова `fibonacci()` для каждого уникального значения аргумента.

В Python 3.8 и выше, если мы не передаем никаких аргументов, то можно использовать декоратор `lru_cache` без скобок. В более ранних версиях необходимо добавить круглые скобки: `lru_cache()`.

### Аргументы декоратора lru_cache

При декорировании функций с помощью декоратора `lru_cache` мы можем использовать следующие аргументы:

- `maxsize=128` — максимальный размер кэша (тип `int`)
- `typed=False` — как кэшировать при разных типах аргументов (тип `bool`)

Если для параметра `maxsize` установлено значение `None`, то кэш может расти без ограничений.

Если для `typed` задано значение `True`, то результаты вызовов функции для аргументов разных типов будут кэшироваться отдельно. Например, `f(3)` и `f(3.0)` будут рассматриваться как отдельные вызовы с разными результатами. Если для `typed` задано значение `False`, то вызовы будут рассматриваться как одинаковые.

Приведенный ниже код:

```python
from functools import lru_cache

@lru_cache(typed=False)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 4))
print(concat('beegeek', 5.0))
print(concat('beegeek', 4.0))
```

выводит:

```no-highlight
beegeek 4
beegeek 5.0
beegeek 4
```

Как мы видим, третий вызов функции с аргументами `('beegeek', 4.0)` использует закэшированный результат первого вызова с аргументами `('beegeek', 4)`.

Приведенный ниже код:

```python
from functools import lru_cache

@lru_cache(typed=True)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 4))
print(concat('beegeek', 5.0))
print(concat('beegeek', 4.0))
```

выводит:

```no-highlight
beegeek 4
beegeek 5.0
beegeek 4.0
```

### Дополнительные методы декоратора lru_cache

Чтобы помочь измерить эффективность кэша и настроить параметр `maxsize`, декорированная функция имеет метод `cache_info()`, который возвращает именованный кортеж, показывающий `hits`, `misses`, `maxsize` и `currsize`.

Мы можем использовать информацию, возвращаемую `cache_info()`, чтобы понять, как работает кэш, и настроить его, чтобы найти подходящий баланс между скоростью работы и объемом памяти:

- `hits` – количество значений, которые `lru_cache` вернул непосредственно из памяти, поскольку они присутствовали в кэше
- `misses` – количество значений, которые были вычислены, а не взяты из памяти
- `maxsize` – это размер кэша, который мы определили, передав его декоратору
- `currsize` – текущий размер кэша

Приведенный ниже код:

```python
from functools import lru_cache

@lru_cache(typed=False)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 1))
print(concat('beegeek', 1.0))
print(concat('beegeek', True))
print(concat('beegeek', 4.0))
print(concat('beegeek', 5))

print(concat.cache_info())
```

выводит:

```no-highlight
beegeek 1
beegeek 1
beegeek 1
beegeek 4.0
beegeek 5
CacheInfo(hits=2, misses=3, maxsize=128, currsize=3)
```

Декоратор `lru_cache` также предоставляет метод `cache_clear()` для очистки кэша.

Приведенный ниже код:

```python
from functools import lru_cache

@lru_cache(typed=False)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 1))
print(concat('beegeek', 1.0))
print(concat('beegeek', True))
print(concat('beegeek', 4.0))
print(concat('beegeek', 5))

print(concat.cache_info())
concat.cache_clear()
print(concat.cache_info())
```

выводит:

```no-highlight
beegeek 1
beegeek 1
beegeek 1
beegeek 4.0
beegeek 5
CacheInfo(hits=2, misses=3, maxsize=128, currsize=3)
CacheInfo(hits=0, misses=0, maxsize=128, currsize=0)
```
