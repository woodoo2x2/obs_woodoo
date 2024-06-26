## Method Resolution Order (MRO)

В Python существует строгий порядок, в котором просматриваются классы во время поиска конкретного метода в иерархии классов при наследовании. Этот порядок определяется для каждого класса индивидуально в зависимости от его иерархии и называется **Method Resolution Order** или сокращенно **MRO**.

Для получения MRO класса достаточно воспользоваться методом `mro()`. Его результатом является список родителей класса, включая сам класс, расположенных ровно в том порядке, в котором Python будет производить поиск методов.

Приведенный ниже код:

```python
class A:
    def method(self):
        print('Метод класса A')
        
class B(A):
    def method(self):
        print('Метод класса B')
        
class C(A):
    def method(self):
        print('Метод класса C')

class D(B, C):
    pass


print(D.mro())
```

выводит:

```no-highlight
[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
```

 MRO класса также можно получить с помощью атрибута класса `__mro__`.

Определив MRO класса, мы всегда можем точно сказать, какой из методов будет унаследован этим классом в том или ином случае, так как знаем, в каком порядке будут проверяться его родительские классы при поиске метода. Так, в нашей ромбовидной иерархии классов `A, B, C` и `D` мы видим, что в MRO класса `D` класс `B` находится перед классами `A` и `C`, следовательно, определенный во всех трех родительских классах метод `method()` будет унаследован именно из класса `B`.

Например, если метод `method()` будет определен только в классах `A` и `C`, то унаследованный метод будет принадлежать классу `C`, потому что в MRO класса `D` класс `C` находится перед классом `A`.

Приведенный ниже код:

```python
class A:
    def method(self):
        print('Метод класса A')
        
class B(A):
    pass
        
class C(A):
    def method(self):
        print('Метод класса C')

class D(B, C):
    pass


d = D()

d.method()
```

выводит:

```no-highlight
Метод класса C
```

Аналогично, если метод `method()` будет определен только в классе `A`, то унаследованный метод будет принадлежать именно этому классу.

Приведенный ниже код:

```python
class A:
    def method(self):
        print('Метод класса A')
        
class B(A):
    pass
        
class C(A):
    pass

class D(B, C):
    pass


d = D()

d.method()
```

выводит:

```no-highlight
Метод класса A
```

Наконец, если метод `method()` будет определен во всех родительских классах, включая сам класс `D`, то использоваться будет метод, реализованный именно в классе `D`.

Приведенный ниже код:

```python
class A:
    def method(self):
        print('Метод класса A')
        
class B(A):
    def method(self):
        print('Метод класса B')
        
class C(A):
    def method(self):
        print('Метод класса C')

class D(B, C):
    def method(self):
        print('Метод класса D')


d = D()

d.method()
```

выводит:

```no-highlight
Метод класса D
```

MRO решает основную проблему множественного наследования — неоднозначность, возникающую при ромбовидном наследовании. Зная MRO класса, мы всегда можем определить, какой метод будет унаследован этим классом, независимо от сложности его иерархии.

  В MRO любого класса на первом месте всегда указывается исходный класс, а на последнем — класс `object`.