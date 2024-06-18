Функция `issubclass()` позволяет проверить, является ли класс прямым или косвенным наследником другого класса.

Приведенный ниже код:

```python
class A:
    pass

class B(A):
    pass

class C(B):
    pass


print(issubclass(A, A))
print(issubclass(A, C))
print(issubclass(B, A))
print(issubclass(B, C))
print(issubclass(C, A))
print(issubclass(C, B))
```

выводит:

```no-highlight
True
False
True
False
True
True
```

Обратите внимание, что любой класс является наследником самого себя.