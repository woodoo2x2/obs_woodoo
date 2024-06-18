

Когда мы только начинали знакомство с классами, мы могли обратить внимание, что создаваемые нами пустые классы на самом деле не полностью пусты и по умолчанию обладают определенным функционалом. Дело в том, что все классы в Python являются наследниками класса `object`.

Приведенный ниже код:

```python
class MyClass:
   pass


print(dir(MyClass()))
print(dir(object()))
```

выводит:

```no-highlight
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
```

Как мы видим, списки атрибутов и методов экземпляров нашего пустого класса и класса `object` практически одинаковы, лишь с тем различием, что экземпляр класса `MyClass` имеет три дополнительных атрибута в виде `__dict__, __module__` и `__weakref__`.

Так как любой класс в Python неявно наследуется от класса `object`, определения классов `class MyClass(object)` и `class MyClass` полностью равнозначны.