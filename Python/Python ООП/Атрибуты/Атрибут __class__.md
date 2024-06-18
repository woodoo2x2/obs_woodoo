Тип любого объекта можно получить через атрибут `__class__`.

Приведенный ниже код:

```python
class Cat:
    pass


cat = Cat()

print(type(cat))
print(cat.__class__)
print(type(cat) == cat.__class__)
```

выводит:

```no-highlight
<class '__main__.Cat'>
<class '__main__.Cat'>
True
```