Синтаксис:

```python
reversed(seq)
```
Параметры:

- итерируемый объект.

Возвращаемое значение:

обратный итератор.

Описание:

Функция reversed() возвращает обратный итератор, то есть возвращает итератор, который перебирает элементы оригинала в обратном порядке.
Функция reversed() не создает копию и не изменяет оригинал последовательности.

Объект seq должен иметь метод __reversed__() или поддерживает протокол последовательности, это метод __len__() и метод __getitem__() с целочисленными аргументами, начинающимися с 0.

Примеры реверса различных последовательностей.
Переворачиваем список;
Переворачиваем строку (кортеж);
Короткая запись реверса строки;

Перевернем список (реверс списка):
```python
>>> x = [15, 11, 13, 12, 14, 10] 
>>> x =list(reversed(x))
>>> x
# [10, 14, 12, 13, 11, 15]

# теперь в обратную сторону
>>> [i for i in reversed(x)]
# [15, 13, 14, 11, 12, 10]

```


Перевернем строку (реверс строки) с помощью reversed():
Так как строка является частным случаем кортежа, а функция reversed() возвращает итератор оригинала последовательности, то исходную строку преобразовывать в список (кортеж) не обязательно.
```python
x = 'forest'

for i in reversed(x):
    # вывод символов строки 'x' 
    # по одному в обратном порядке
    print(i, end='')

print('\n' + '-'*len(x))
print(x)
# tserof
# ------
# forest
```
Если в итоге нужно снова получить строку, только перевернутую (запишем покороче):
```python
>>> x = 'абракадабра'
>>> line = ''.join(reversed(x))
>>> line
# 'арбадакарба'
```