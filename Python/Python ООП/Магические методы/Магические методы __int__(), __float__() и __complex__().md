

В Python объекты одних типов можно преобразовывать в объекты других типов. Например, строку можно преобразовать в целое число или целое число преобразовать в вещественное.

Приведенный ниже код:

```python
print(int('10'))
print(int(10.1))
print(float(10))
print(float('10.1'))
print(complex('1+5j'))
```

выводит:

```no-highlight
10
10
10.0
10.1
(1+5j)
```

Преобразовывать экземпляры собственных классов в объекты типа `int, float` и `complex` можно, определив в классе ряд магических методов:

- `__int__()` — определяет поведение экземпляра при передаче в [[Функция int()]]. Метод должен возвращать значение, соответствующее преобразованию экземпляра в [[Тип данных int]]
- `__float__()` — определяет поведение экземпляра при передаче в функцию [[Функция float()]]. Метод должен возвращать значение, соответствующее преобразованию экземпляра в тип [[Тип данных float]]
- `__complex__()` — определяет поведение экземпляра при передаче в функцию [[Функция complex()]]. Метод должен возвращать значение, соответствующее преобразованию экземпляра в тип [[Тип данных complex]]

Определим в классе `Angle` методы `__int__()` и `__float__()`, которые позволят преобразовывать экземпляры этого класса в целые и вещественные числа соответственно.

Приведенный ниже код:

```python
class Angle:
    def __init__(self, value):
        self.value = value

    def __int__(self):
        return int(self.value)

    def __float__(self):
        return float(self.value)


angle1 = Angle(100)
angle2 = Angle(100.1)

print(int(angle1))
print(int(angle2))
print(float(angle1))
print(float(angle2))
```

выводит:

```no-highlight
100
100
100.0
100.1
```