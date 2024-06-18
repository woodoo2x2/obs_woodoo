Синтаксис:

```python
isinstance(object, classinfo)
```

Параметры:

- object - объект, требующий проверки,
- classinfo - класс, кортеж с классами или рекурсивный кортеж кортежей или с версии Python 3.10 может быть объединением нескольких типов (например int | str).

Возвращаемое значение:

  [[bool (Логический тип данных)]]

Описание:

Функция isinstance() вернет True, если проверяемый объект object является экземпляром любого указанного в classinfo класса (классов) или его подкласса (прямого, косвенного или виртуального).

Если объект object не является экземпляром данного типа, то функция всегда возвращает False.

Функцией isinstance() можно проверить класс, кортеж с классами (с Python 3.10 может быть записью, объединяющей нескольких типов, например, int | str), либо рекурсивный кортеж кортежей. Другие типы последовательностей аргументом classinfo не поддерживаются.


```python
>>> isinstance(1, int | str)
# True
>>> isinstance("", int | str)
# True
```

Доступ к пользовательскому типу для объекта union можно получить из types.UnionType и использовать для проверок isinstance(). Невозможно создать экземпляр объекта из типа:

```python

>>> import types
>>> isinstance(int | str, types.UnionType)
# True
>>> types.UnionType()
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: cannot create 'types.UnionType' instances
```

Функция `isinstance()` позволяет проверить, является ли объект прямым или косвенным экземпляром некоторого класса.

Приведенный ниже код:

```python
class A:
    pass

class B(A):
    pass

class C(B):
    pass


obj = C()

print(isinstance(obj, A))
print(isinstance(obj, B))
print(isinstance(obj, C))
```

выводит:

```no-highlight
True
True
True
```

Так как данная функция при проверке учитывает иерархию классов, при проверке на принадлежность классу рекомендуется использовать именно ее.