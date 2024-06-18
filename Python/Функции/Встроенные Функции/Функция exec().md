

Функция `exec()`, в отличие от [[Функция eval()|eval()]], принимает блок кода и выполняет его, возвращая значение [[None (NoneType)|None]]. Аргумент функции:

- `code` — строка, представляющая собой корректный блок кода

Приведенный ниже код:

```python
code = '''a = 10
b = 20
print(a + b)'''

exec(code)
```

выводит:

```no-highlight
30
```

Важно понимать, что функция `exec()` именно выполняет переданный блок кода и всегда возвращает значение `None`.

Приведенный ниже код:

```python
code = '100 + 10*7 - 14'

result = exec(code)

print(result)
```

выводит:

```no-highlight
None
```

Блок кода, передаваемый в качестве аргумента функции `exec()`, имеет доступ ко всем встроенным функциям Python.

```python
code1 = 'print(sorted([3, 5, 4, 1, 2]))'
code2 = 'print(sum([3, 5, 4, 1, 2]))'
code3 = 'print(len([3, 5, 4, 1, 2]))'

exec(code1)
exec(code2)
exec(code3)
```

выводит:

```python
[1, 2, 3, 4, 5]
15
5
```

Блок кода, передаваемый в качестве аргумента функции `exec()`, имеет доступ ко всем локальным и глобальным переменным.

Приведенный ниже код:

```python
numbers = [1, 2, 3, 4, 5]
info = {'name': 'Timur', 'surname': 'Guev'}

code1 = '''total = 0
for i in numbers:
    total += i
print(total)'''
code2 = 'print(info["name"], info["surname"])'

exec(code1)
exec(code2)
```

выводит: 

```no-highlight
15
Timur Guev
```
