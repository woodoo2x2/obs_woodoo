

Свойства являются [[Атрибуты класса|атрибутами классов]], которые доступны всем экземплярам этих классов. Обращаясь к свойству через экземпляр класса, мы работаем с ним как с атрибутом, неявно вызывая соответствующие методы ([[Геттеры]], [[Сеттеры]] и [[Делитеры]]). При этом само свойство является обычным [[Объект|объектом]], и мы можем получить к нему доступ через класс, в котором это свойство определено.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


print(Cat.name)
print(type(Cat.name))
```

выводит (адрес может отличаться):

```no-highlight
<property object at 0x000001829C8F56C0>
<class 'property'>
```

Таким образом, свойство — это атрибут класса, который управляет атрибутами экземпляров. Мы также можем считать свойство набором методов (геттер, сеттер, делитер), собранных вместе в единый объект. Обратиться к методам, содержащимся в свойстве, можно через атрибуты `fget, fset` и `fdel`.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


print(Cat.name.fget)                                   # обращаемся к геттеру свойства
print(Cat.name.fset)                                   # обращаемся к сеттеру свойства
print(Cat.name.fdel)                                   # обращаемся к делитеру свойства
```

выводит (адрес может отличаться):

```no-highlight
<function Cat.get_name at 0x000001F8C2FC1090>
<function Cat.set_name at 0x000001F8C2FC1120>
None
```

`property` — это класс, предназначенный для работы как функция, а не как обычный класс, поэтому большинство разработчиков называют ее функцией. По этой же причине [[Функция property()|property()]] не соответствует соглашению Python об именовании классов.