## Полезные типы модуля typing

Многие типы из модуля `typing` позволяют работать с несколькими типами одновременно. К наиболее часто используемым типам модуля `typing` относятся:

- `Union`
- `Optional`
- `Any`
- `NoReturn`

Рассмотрим каждый из них подробнее.

### Тип Union

Часто возникает ситуация, когда нужно объединить несколько типов, например, для того, чтобы указать, что функция может принимать и целые и вещественные числа. Этого можно достичь при помощи специального типа `Union` из модуля `typing`.

Функция `add_or_concatenate()` должна принимать аргументы типов `int, float, str` и возвращает один из трех типов `int, float, str`:

```python
from typing import Union

def add_or_concatenate(a: Union[int, float, str], b: Union[int, float, str]) -> Union[int, float, str]:
    return a + b
```

Приведенный выше код можно записать в виде:

```python
from typing import Union

NumberOrStr = Union[int, float, str]

def add_or_concatenate(a: NumberOrStr, b: NumberOrStr) -> NumberOrStr:
    return a + b
```

Особенности типа `Union`:

1. аргументы указываемые в квадратных скобках должны быть типами, причем необходимо указать хотя бы один тип
2. запись `Union[Union[int, str], float]` эквивалентна записи `Union[int, str, float]`
3. запись `Union[int]` эквивалентна записи `int`
4. запись `Union[int, str, int]` эквивалентна записи `Union[int, str]`
5. запись `Union[int, str]` эквивалентна записи `Union[str, int]`

### Тип Optional

Также очень часто возникает ситуация, когда возможно либо значение определенного типа, либо `None`. Мы можем использовать тип `Union` следующим образом:

```python
from typing import Union

name: Union[str, None]
```

Однако это настолько частая ситуация, что для этого даже сделали отдельный тип `Optional`:

```python
from typing import Optional

name: Optional[str]
```

По сути `name: Union[str, None]` и `name: Optional[str]` это одно и то же, но второй вариант читается проще.

### Тип Any

Может возникнуть ситуация, когда не получается указать какой-либо конкретный тип, потому что, функция может принимать на вход абсолютно что угодно. Для этих случаев тоже есть специальный тип `Any`:

```python
from typing import Any

def func(arg: Any) -> Any:
    return arg
```

Таким образом любой тип совместим с типом `Any`, как и `Any` совместим с любым типом.

![](https://ucarecdn.com/0a5e06fa-6393-49ab-9ec7-5741d96f7bb4/)Очень соблазнительно везде вставлять тип `Any`, но настоятельно рекомендуется использовать его только в крайних случаях, потому что чрезмерное его использование сводит пользу от аннотаций типов на нет.

### Тип NoReturn

Специальный тип `NoReturn` указывает, что функция никогда не возвращает значение:

```python
from typing import NoReturn

def stop() -> NoReturn:
    raise RuntimeError('no way')
```

## Примечания

**Примечание 1.** Документация модуля `typing` доступна по [ссылке](https://docs.python.org/3/library/typing.html).
