В Python для создания экземпляра класса достаточно вызвать данный класс как функцию и передать ему соответствующий набор аргументов, если это необходимо.

Приведенный ниже код:

```python
class MyClass:
    pass


obj = MyClass()

print(obj)
print(type(obj))
```

создает экземпляр класса `MyClass`, присваивает его переменной `obj` и выводит (адрес может отличаться):

```no-highlight
<__main__.MyClass object at 0x000001FCABBE6560>
<class '__main__.MyClass'>
```

Когда мы вызываем класс, мы запускаем внутренний процесс **конструирования экземпляра класса**, который состоит из двух шагов:

1. создание нового пустого экземпляра класса
2. инициализация созданного экземпляра класса

Для выполнения первого шага все классы имеют магический метод `__new__()`, который отвечает за создание и возврат нового пустого экземпляра класса. Затем созданный экземпляр передается в метод [[Магический метод __init__()|__init__()]] для инициализации, то есть для установки его атрибутам необходимых значений.

Мы уже хорошо знакомы с процессом инициализации и методом `__init__()`, который в качестве первого аргумента [[Параметр self|self]] принимает уже созданный экземпляр класса, однако его создание всегда происходило без нашего ведома.

Непосредственное создание объекта происходит с помощью метода `__new__()`. Данный метод вызывается самым первым при конструировании объекта. Метод `__new__()` принимает в качестве первого обязательного аргумента класс, а затем, как правило, произвольное количество позиционных и именованных аргументов.

Приведенный ниже код:

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        instance = object.__new__(cls)
        return instance


obj = MyClass()

print(obj)
print(type(obj))
```

выводит (адрес может отличаться):

```no-highlight
<__main__.MyClass object at 0x000002A321CF6C20>
<class '__main__.MyClass'>
```

В примере выше метод `__new__()` создает экземпляр класса `MyClass`, присваивает его переменной `instance` и возвращает данный экземпляр.

Для понимания процесса создания экземпляра стоит упомянуть важную деталь: в Python существует класс под названием `object`, который является родительским абсолютно для всех классов. Мы будем подробнее говорить об этом в модуле Наследование, однако сейчас предлагаем просто помнить об этом.

Класс `object` имеет множество базовых методов, одним из которых является метод `__new__()`, отвечающий за создание всех объектов в Python. Данный метод принимает в качестве аргумента класс и возвращает экземпляр этого класса.

Таким образом, конструкция `object.__new__(cls)` позволяет нам обратиться к методу `__new__()` класса `object` и создать экземпляр класса [[Параметр cls|cls]], в нашем случае класса `MyClass`.

### Особенности метода __new__()

Первым обязательным аргументом метода `__new__()` пользовательского класса является сам класс, после которого, как правило, следуют произвольное количество позиционных и именованных аргументов. Дело в том, что аргументы, указываемые при вызове класса, передаются как в метод `__init__()`, так и в метод `__new__()`.

Приведенный ниже код:

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        print(args, kwargs)
        instance = object.__new__(cls)
        return instance


obj = MyClass(1, 2, c=3, d=4)
```

выводит: 

```no-highlight
(1, 2) {'c': 3, 'd': 4}
```

Для большей гибкости метод `__new__()` всегда рекомендуется определять именно с произвольным количеством позиционных и именованных параметров.

Вместо явного обращения к методу `__new__()` родительского класса `object` можно использовать специальную функцию [[Функция super()]].

Приведенный ниже код:

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        instance = object.__new__(cls)
        return instance
```

равнозначен коду:

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        instance = super().__new__(cls)
        return instance
```

Метод `__new__()` всегда должен возвращать экземпляр того класса, в котором этот метод определен. В противном случае метод `__init__()` вызываться не будет.

Приведенный ниже код:

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        print('1. Создание экземпляра класса MyClass')
        instance = 'instance'
        return instance                                 # возвращаем экземпляр класса str, а не MyClass

    def __init__(self):
        print('2. Инициализация созданного экземпляра класса MyClass')


obj = MyClass()
```

выводит:

```no-highlight
1. Создание экземпляра класса MyClass
```

При этом метод `__init__()` всегда должен возвращать значение [[None (NoneType)]].

Приведенный ниже код:

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        print('1. Создание экземпляра класса MyClass')
        instance = object.__new__(cls)
        return instance

    def __init__(self):
        print('2. Инициализация созданного экземпляра класса MyClass')
        return self


obj = MyClass()
```

приводит к возбуждению исключения:

```no-highlight
TypeError: __init__() should return None, not 'MyClass'
```