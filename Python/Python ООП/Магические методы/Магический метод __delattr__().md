

Метод `__delattr__()` вызывается при удалении атрибута.

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed
    
    def __delattr__(self, attr):
        print(f'Удаляю атрибут {attr}')
        del self.__dict__[attr]


cat = Cat('Кемаль', 'Британский')

del cat.name
print(cat.__dict__)

del cat.breed
print(cat.__dict__)
```

выводит:

```no-highlight
Удаляю атрибут name
{'breed': 'Британский'}
Удаляю атрибут breed
{}
```

Аналогично методу [[Магический метод __setattr__()]], удаление атрибута внутри метода `__delattr__()` происходит напрямую через словарь атрибутов [[Атрибут __dict__]].

Вместо обращения к словарю атрибутов `__dict__`, метод `__delattr__()` может использовать свою базовую реализацию из класса [[object (Базовый класс)]].

Приведенный ниже код:

```python
class Cat:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed
    
    def __delattr__(self, attr):
        print(f'Удаляю атрибут {attr}')
        object.__delattr__(self, attr)


cat = Cat('Кемаль', 'Британский')

del cat.name
print(cat.__dict__)

del cat.breed
print(cat.__dict__)
```

выводит:

```no-highlight
Удаляю атрибут name
{'breed': 'Британский'}
Удаляю атрибут breed
{}
```