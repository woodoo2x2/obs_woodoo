Атрибут класса `__base__` позволяет получить родительский класс текущего класса.

Приведенный ниже код:

```python
class A:
    pass

class B(A):
    pass

class C(B):
    pass


print(A.__base__)
print(B.__base__)
print(C.__base__)
```

выводит:

```no-highlight
<class 'object'>
<class '__main__.A'>
<class '__main__.B'>
```