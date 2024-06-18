
Значения, получаемые при передаче объектов в такие функции как [[Функция abs()]] [[Функция round()]], trunc(), floor() и ceil(), так же определяются магическими методами:

- [[Магический метод __abs__]] — определяет поведение для встроенной функции `abs()`
- [[Магический метод __round__]] — определяет поведение для встроенной функции `round()`; помимо экземпляра класса метод принимает необязательный аргумент `n`, который, как правило, означает количество знаков после запятой после округления
- [[Магический метод __trunk__]] — определяет поведение для функции `trunc()` из модуля `math`
- [[Магический метод __floor__]] — определяет поведение для функции `floor()` из модуля `math`
- [[Магический метод __ceil__]] — определяет поведение для функции `ceil()` из модуля `math`

Приведенный ниже код:

```python
import math

class Angle:
    def __init__(self, value):
        self.value = value

    def __repr__(self):
        return f'Angle({self.value})'

    def __abs__(self):
        return Angle(abs(self.value))

    def __round__(self, n=None):
        if n is None:
            return Angle(round(self.value))
        return Angle(round(self.value, n))

    def __trunc__(self):
        return Angle(math.trunc(self.value))

    def __floor__(self):
        return Angle(math.floor(self.value))

    def __ceil__(self):
        return Angle(math.ceil(self.value))


angle = Angle(-101.54)

print(abs(angle))
print(round(angle))
print(round(angle, 1))
print(math.trunc(angle))
print(math.floor(angle))
print(math.ceil(angle))
```

выводит:

```no-highlight
Angle(101.54)
Angle(-102)
Angle(-101.5)
Angle(-101)
Angle(-102)
Angle(-101)
```

Здесь функции `abs(), round(), trunc(), floor()` и `ceil()` возвращают новые экземпляры класса `Angle`. Например, функция `abs()` возвращает угол, градусная мера которого взята по модулю. Оставшиеся функции возвращают углы, градусные меры которых округлены соответствующим образом.