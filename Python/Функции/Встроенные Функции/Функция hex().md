Синтаксис:
```
hex(x)
```
Параметры:
- x -  [[Тип данных int]]

Возвращаемое значение:

шестнадцатеричная [[Тип данных str|строка]] с префиксом 0x.
Описание:

Функция hex() преобразует целое число в шестнадцатеричную строку с префиксом 0x.

В функцию hex() может быть передан любой объект, реализующий метод __index__(), возвращающий целое число.

Если вы хотите преобразовать целое число в шестнадцатеричную строку с префиксом в верхнем или нижнем регистре, можете использовать один из следующих способов:

```py
>>> '%#x' % 255, '%x' % 255, '%X' % 255
('0xff', 'ff', 'FF')
>>> format(255, '#x'), format(255, 'x'), format(255, 'X')
('0xff', 'ff', 'FF')
>>> f'{255:#x}', f'{255:x}', f'{255:X}'
('0xff', 'ff', 'FF')
```
Смотрите также [[Функция format()]]  для получения дополнительной информации.

Смотрите также [[Функция int()]] для преобразования шестнадцатеричной строки в целое число, по основанию 16.

Заметка:
Чтобы получить шестнадцатеричное строковое представление для числа с плавающей запятой, используйте метод float.hex().

Примеры преобразований числа в шестнадцатеричную строку.
```py
>>> hex(255)
'0xff'
>>> hex(-42)
'-0x2a'
```