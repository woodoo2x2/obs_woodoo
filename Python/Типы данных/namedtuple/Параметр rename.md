

Допустим, мы импортируем данные из CSV-файла и превращаем каждую строку в именованный кортеж. Структура файла имеет вид:

```no-highlight
name,surname,age,class
Timur,Guev,28,11
Ruslan,Chaniev,22,9
...
```

Названия полей мы берем из заголовка CSV-файла:

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class')

Student = namedtuple('Student', headers)
```

Поскольку одно поле имеет название `class` (ключевое слово языка Python) мы получаем ошибку: `ValueError: Type names and field names cannot be a keyword: 'class'`.

Проблема заключается в том, что мы не знаем, будут ли в качестве названий полей у нас ключевые слова языка Python или нет. Для решения данной проблемы можно использовать параметр `rename` со значением `True`.

Приведенный ниже код:

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class')

Student = namedtuple('Student', headers, rename=True)

stud = Student('Роман', 'Белых', 26, 10)
print(stud)
```

выводит:

```no-highlight
Student(name='Роман', surname='Белых', age=26, _3=10)
```

Обратите внимание на то, что Python автоматически переименовал поле `class` в `_3`.

Приведенный ниже код:

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class', 'with', 'color', 'name', 'class', 'if')

Student = namedtuple('Student', headers, rename=True)

stud = Student('Тимур', 'Гуев', 28, 11, 'sister', 'green', 'Tim', '11A', 'else')
print(stud)
```

выводит:

```no-highlight
Student(name='Тимур', surname='Гуев', age=28, _3=11, _4='sister', color='green', _6='Tim', _7='11A', _8='else')
```

Как мы видим, неудачные имена полей переименовались в соответствии с их порядковыми номерами, причем перед порядковым номером используется символ подчеркивания.