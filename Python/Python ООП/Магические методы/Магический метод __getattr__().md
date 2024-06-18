

Как уже было сказано, метод `__getattr__()` вызывается только в двух случаях:

- если в теле метода [[Магический метод __getattribute__()]] было возбуждено исключение `AttributeError`
- если метод `__getattr__()` был явно вызван в теле метода `__getattribute__()`

Применять метод `__getattr__()` можно по-разному. Cамый простой вариант — возврат значения по умолчанию при обращении к несуществующему атрибуту, вместо привычного возбуждения исключения `AttributeError`.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self.name = name
        
    def __getattr__(self, attr):
        print(f'Возвращаю значение по умолчанию')
        return None


cat = Cat('Кемаль')

print(cat.name)                                         # обращение к существующему атрибуту
print(cat.age)                                          # обращение к несуществующему атрибуту
print(cat.breed)                                        # обращение к несуществующему атрибуту
```

выводит:

```no-highlight
Кемаль
Возвращаю значение по умолчанию
None
Возвращаю значение по умолчанию
None
```

Также с помощью метода `__getattr__()` можно имитировать наличие различных атрибутов, которыми объект на самом деле не обладает.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed                              # порода кошки
        
    def __getattr__(self, attr):
        if attr == 'info':
            return f'Имя: {self.name}\nПорода: {self.breed}'
        raise AttributeError


cat = Cat('Кемаль', 'Британский')

print(cat.info)                                         # обращение к несуществующему атрибуту
```

выводит:

```no-highlight
Имя: Кемаль
Порода: Британский
```

У многих наверное возник вопрос, где это можно применить?!

Этот механизм может быть полезен при реализации динамического поведения объектов или атрибутов, создаваемых на лету

**1. Доступ к свойствам объекта по ключу словаря**

```python
class Person:
    def __init__(self):
        self.data = {"name": "John", "age": 30}

    def __getattr__(self, attr):
        if attr in self.data:
            return self.data[attr]
        raise AttributeError(f"'Person' object has no attribute '{attr}'")

person = Person()

# Обращение к несуществующим атрибутам
print(person.name)  	# John
print(person.age)   	# 30
print(person.height)	# AttributeError: 'Person' object has no attribute 'height'
```

**2. Имитация доступа к несуществующим методам**

 Класс CustomMath имитирует доступ к методам "sqrt_", вычисляющий квадратный корень числа  
 Если вызывается метод, начинающийся не с "sqrt_", то возникает исключение AttributeError.

```python
class CustomMath:
    def __getattr__(self, attr):
        if attr.startswith("sqrt_"):
            number = float(attr.split("_")[1])
            return lambda: number ** 0.5
        raise AttributeError(f"'CustomMath' object has no attribute '{attr}'")

math_obj = CustomMath()
print(math_obj.sqrt_16())  # 4.0
print(math_obj.sqrt_25())  # 5.0
print(math_obj.sin_30())   # AttributeError: 'CustomMath' object has no attribute 'sin_30
```