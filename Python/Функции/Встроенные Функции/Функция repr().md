Синтаксис:

```python
repr(object)
```

Параметры:

- object - объект языка Python.

Возвращаемое значение:

- формальное строковое представление.

Описание:

Функция repr() вернет строку, содержащую печатаемое формальное представление объекта.

Для многих типов функция возвращает строку, которая при передаче в eval() может произвести объект с тем же значением, что и исходный. В других случаях представление является строкой, обрамлённой угловыми скобками (< и >), содержащей название типа и некую дополнительную информацию, часто название объекта и его адрес в памяти.

Чтобы определить значение, возвращаемое функцией для пользовательского типа следует реализовать для этого типа специализированный метод __repr__.
Примеры получения описания объекта.

```python
class Person:
    name = 'Mike'


x = Person()
print(repr(x))
# <__main__.Person object at 0x7f0c483edbe0>


# Определим метод __repr__
class Person:
    name = 'Mike'

    def __repr__(self):
        return repr(self.name)


x = Person()
print(repr(x))
# 'Mike'
```
Другие примеры:
```python
>>> x = [1, 2, 3, 4, 5, 6, 7]
>>> repr(x)
# '[1, 2, 3, 4, 5, 6, 7]'
>>> repr('Hello')
# "'Hello'"
>>> x = ('a', 'b', 'c', 'd')
>>> repr(x)
# "('a', 'b', 'c', 'd')"
>>> x = range(5)
>>> repr(x)
# 'range(0, 5)'
>>>
```

