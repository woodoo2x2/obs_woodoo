Синтаксис:
```py
next(iterator, default)
```
Параметры:
- iterator - объект итератора, в котором определен метод __next__(),
- default - значение по умолчанию, которое будет возвращено вместо исключения StopIteration.

Возвращаемое значение:
- следующий элемент итераторa.

Описание:
Функция next() возвращает следующий элемент итератора, вызвав его метод __next__().

Если итератор исчерпан:

бросается исключение StopIteration, если значение по умолчанию default не задано;
возвращается значение default, если оно задано
Для создания объекта итератора можно воспользоваться функцией iter().

Примеры извлечения следующего элемента итератора.
```py
def fruit_generate():
    # Создадим итератор при помощи генератора.
    for item in ['apple', 'banana', 'cherry']:
        yield item

fruit = fruit_generate()

print(next(fruit))
# apple
print(next(fruit))
# banana
print(next(fruit))
# cherry
print(next(fruit))
# Traceback (most recent call last):
#  File "/home/script/next-fruit.py", line 15, in <module>
#    print(next(fruit))
# StopIteration
```
Вернем значение по умолчанию, когда итерация достигнет конца:

```py
# Создадим итератор при помощи функции iter().
fruit = iter(['apple', 'banana', 'cherry'])

print(next(fruit, 'STOP'))
# apple
print(next(fruit, 'STOP'))
# banana
print(next(fruit, 'STOP'))
# cherry
print(next(fruit, 'STOP'))
# STOP
```