

Хранить строки в переменных удобно, но часто бывает необходимо собирать строки из других объектов (строк, чисел и т.д.) и выполнять с ними нужные манипуляции. Для этой цели можно воспользоваться механизмом **форматирования строк**.

Рассмотрим следующий код:

```python
birth_year = 1992
text = 'My name is Timur, I was born in ' + birth_year

print(text)
```

Такой код приводит к ошибке во время выполнения программы, поскольку мы пытаемся сложить число и строку:

```no-highlight
TypeError: can only concatenate str (not "int") to str
```

Для решения такой проблемы мы можем использовать функцию `str()`, которая преобразует числовое значение в строку.

Приведенный ниже код:

```python
birth_year = 1992
text = 'My name is Timur, I was born in ' + str(birth_year)

print(text)
```

выводит:

```no-highlight
My name is Timur, I was born in 1992
```

Такой код работает, но каждый раз преобразовывать число в строку не очень удобно. Для более наглядного форматирования мы можем использовать строковый метод `format()`.

Предыдущий код можно переписать в виде:

```python
birth_year = 1992
text = 'My name is Timur, I was born in {}'.format(birth_year)

print(text)
```

Мы передаем необходимые параметры методу `format()`, а Python ставит их вместо фигурных скобок `{}` – **заполнителей**. Мы можем создавать сколько угодно заполнителей.

Приведенный ниже код:

```python
birth_year = 1992
name = 'Timur'
profession = 'math teacher'
text = 'My name is {}, I was born in {}, I work as a {}.'.format(name, birth_year, profession)

print(text)
```

выводит:

```no-highlight
My name is Timur, I was born in 1992, I work as a math teacher.
```

Для наглядности и гибкости форматирования мы можем использовать порядковый номер в заполнителе: `{0}`, `{1}`, `{2}` и т.д. Такой номер определяет позицию параметра, переданного методу `format()` (нумерация начинается с нуля):

```python
birth_year = 1992
name = 'Timur'
profession = 'math teacher'
text = 'My name is {0}, I was born in {1}, I work as a {2}.'.format(name, birth_year, profession)

print(text)
```

Параметр `name` встает в заполнителе `{0}`, параметр `birth_year` – в заполнителе `{1}` и т.д. Мы можем использовать одно и то же число в нескольких заполнителях или не использовать совсем, а также мы можем использовать числа в разном порядке.

Приведенный ниже код:

```python
name = 'Timur'
city = 'Moscow'
text1 = 'My name is {0}-{0}-{0}!'.format(name, city)
text2 = '{1} is my city and {0} is my name!'.format(name, city)

print(text1)
print(text2)
```

выводит:

```no-highlight
My name is Timur-Timur-Timur!
Moscow is my city and Timur is my name!
```