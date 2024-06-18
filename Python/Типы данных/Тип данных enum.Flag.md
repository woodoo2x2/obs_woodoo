## Битовые флаги

Помимо класса `Enum` в модуле `enum` содержится класс `Flag`, с помощью которого можно создавать битовые флаги. Битовые флаги похожи на перечисления, однако в отличие от перечислений, элементы флагов поддерживают битовые операции с помощью операторов `|` (побитовое ИЛИ), `&` (побитовое И), `^` (побитовое исключающее ИЛИ) и `~` (побитовое НЕ).

В качестве флага рассмотрим класс `Color`, элементами которого являются цвета:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4
```

Первое, на что стоит обратить внимание во флаге `Color`, это на значения его элементов: все они являются **степенями двойки**. Элемент `RED` имеет значение `2^0`, `GREEN` — `2^1`, `BLUE` — `2^2`. Значениями элементов должны являться именно степени двойки для того, чтобы упомянутые выше битовые операции выполнялись корректно. Более очевидным необходимость таких значений становится в том случае, если записать их в двоичной системе счисления:

```python
from enum import Flag

class Color(Flag):
    RED = 1                                      # 001
    GREEN = 2                                    # 010
    BLUE = 4                                     # 100
```

Несложно заметить, что в двоичной записи значение каждого элемента состоит из нулей и одной единицы, причем позиция единицы для каждого значения уникальна. Таким образом, позиция единицы в двоичной записи значения и определяет элемент флага.

## Оператор |

С помощью оператора `|` можно объединить элементы флага. Например, в двоичной записи значения элемента `RED`  единица находится на последней позиции, элемента `GREEN` — на предпоследней позиции. Выполнив операцию побитового ИЛИ для значений элементов `RED` и `GREEN`, мы получим значение `011`, что соответствует объединению элементов `RED` и `GREEN`, так как единица в данном случае находится и на последней позиции, и на предпоследней.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined1 = Color.RED | Color.GREEN
combined2 = Color.RED | Color.GREEN | Color.BLUE

print(combined1)
print(combined2)
```

выводит:

```no-highlight
Color.GREEN|RED
Color.BLUE|GREEN|RED
```

)Представление значений элементов флага в двоичной системе счисления и знание битовых операций позволяет удобно работать с элементами флага. Подробнее ознакомиться с битовыми операциями можно по [ссылке](https://ru.wikipedia.org/wiki/%D0%91%D0%B8%D1%82%D0%BE%D0%B2%D0%B0%D1%8F_%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%86%D0%B8%D1%8F).

Обратите внимание, что объект, полученный в результате какой-либо битовой операции между элементами флага, сам является элементом флага.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined1 = Color.RED | Color.GREEN
combined2 = Color.RED | Color.GREEN | Color.BLUE

print(type(combined1))
print(type(combined2))
```

выводит:

```no-highlight
<flag 'Color'>
<flag 'Color'>
```

Если он не является фактическим элементом флага (определенным внутри класса), то у него нет имени, однако есть значение.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined1 = Color.RED | Color.GREEN              # 011 в двоичной записи и 3 в десятичной
combined2 = Color.RED | Color.BLUE               # 101 в двоичной записи и 5 в десятичной

print(combined1.name)
print(combined1.value)
print(combined2.name)
print(combined2.value)
```

выводит:

```no-highlight
None
3
None
5
```

## Оператор ^

Имея объединение элементов, с помощью оператора `^` можно исключить элемент или несколько элементов из этого объединения.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined = Color.RED | Color.GREEN | Color.BLUE

print(combined ^ Color.RED)                      # исключаем Color.RED
print(combined ^ Color.GREEN)                    # исключаем Color.GREEN
print(combined ^ Color.BLUE)                     # исключаем Color.BLUE
print(combined ^ (Color.BLUE | Color.RED))       # исключаем Color.BLUE и Color.RED
```

выводит:

```no-highlight
Color.BLUE|GREEN
Color.BLUE|RED
Color.GREEN|RED
Color.GREEN
```

## Оператор &

Имея несколько объединений элементов, с помощью оператора `&` их можно пересечь и получить их общие элементы.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined1 = Color.RED | Color.GREEN | Color.BLUE
combined2 = Color.GREEN | Color.BLUE

print(combined1 & combined2)
print(combined1 & Color.RED)
```

выводит:

```no-highlight
Color.BLUE|GREEN
Color.RED
```

## Оператор ~

С помощью оператора `~` можно получить все элементы флага, кроме заданных.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


print(~Color.RED)                                # все элементы Color, кроме Color.RED
print(~Color.GREEN)                              # все элементы Color, кроме Color.GREEN
print(~Color.BLUE)                               # все элементы Color, кроме Color.BLUE
print(~(Color.RED | Color.GREEN))                # все элементы Color, кроме Color.RED и Color.GREEN
```

выводит:

```no-highlight
Color.BLUE|GREEN
Color.BLUE|RED
Color.GREEN|RED
Color.BLUE
```

## Проверка на принадлежность

Имея объединение элементов, проверить наличие в нем конкретного элемента или нескольких элементов можно с помощью оператора `in`.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined1 = Color.RED | Color.GREEN
combined2 = Color.RED | Color.GREEN | Color.BLUE

print(Color.RED in combined1)
print(Color.BLUE in combined1)
print(combined1 in combined2)
```

выводит:

```no-highlight
True
False
True
```

## Нулевой элемент

В результате битовых операций с элементами флага может получиться элемент с нулевым значением. Ошибки в данном случае не произойдет, так как флаг для подобных ситуаций автоматически определяет элемент с именем `None` и значением `0`.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined = ~(Color.RED | Color.GREEN | Color.BLUE)

print(combined)
print(combined.name)
print(combined.value)
```

выводит:

```no-highlight
Color.0
None
0
```

Иногда удобно определить подобный элемент в классе явно.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    NONE = 0
    RED = 1
    GREEN = 2
    BLUE = 4


zero_value = ~(Color.RED | Color.GREEN | Color.BLUE)

print(zero_value)
print(zero_value.name)
print(zero_value.value)
```

выводит:

```no-highlight
Color.NONE
NONE
0
```

## Битовые флаги в Python 3.11

С релизом Python 3.11 класс `Flag` был обновлен. Так, в новой версии языка элемент флага с нулевым значением получил новое строковое представление.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined = ~(Color.RED | Color.GREEN | Color.BLUE)

print(str(combined))
print(repr(combined))
```

в Python 3.10 выводит:

```no-highlight
Color.0
<Color.0: 0>
```

в то время как в Python 3.11 выводит:

```no-highlight
Color(0)
<Color: 0>
```

Элементы флага стали итерируемыми объектами, имеющими длину, которую можно получить с помощью встроенной функции `len()`.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined = Color.RED | Color.GREEN | Color.BLUE

print(len(Color.RED))
print(*Color.RED)
print(len(combined))
print(*combined)
```

в Python 3.10 приводит к возбуждению исключения:

```no-highlight
TypeError: object of type 'Color' has no len()
```

в то время как в Python 3.11 выводит:

```no-highlight
1
Color.RED
3
Color.RED Color.GREEN Color.BLUE
```

Также элементы флага, полученные в результате каких-либо битовых операций и не являющиеся его фактическими элементами, стали обладать именами.

Приведенный ниже код:

```python
from enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


combined1 = Color.RED | Color.GREEN
combined2 = Color.RED | Color.BLUE

print(combined1.name)
print(combined2.name)
```

в Python 3.10 выводит:

```no-highlight
None
None
```

в то время как в Python 3.11 выводит:

```python
RED|GREEN
RED|BLUE
```
 Битовые флаги имеют весь функционал обычных перечислений: доступ к элементам, итерирование, сравнение на равенство и идентичность и т.д.

Приведенный ниже код:

```python
from  enum import Flag

class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4


print(Color.RED)
print(Color(2))
print(Color['BLUE'])

for color in Color:
    print(color.name, color.value)
```

выводит:

```no-highlight
Color.RED
Color.GREEN
Color.BLUE
RED 1
GREEN 2
BLUE 4
```

Для автоматической установки в качестве значений элементам флага степеней двойки можно использовать функцию `auto()`.

Приведенный ниже код:

```python
from enum import Flag, auto

class Color(Flag):
    RED = auto()
    GREEN = auto()
    BLUE = auto()


for color in Color:
    print(color.name, color.value)
```

выводит:

```no-highlight
RED 1
GREEN 2
BLUE 4
```
