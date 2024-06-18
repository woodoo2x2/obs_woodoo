Магический метод `__contains__()` можно считать как частью протокола последовательности, так и нет. Дело в том, что если данный метод не определен, Python самостоятельно перебирает всю последовательность и возвращает `True`, если находит искомый элемент, или `False` в противном случае.

Приведенный ниже код:

```python
class Order:
    def __init__(self, cart, customer):
        self.cart = list(cart)
        self.customer = customer

    def __len__(self):
        return len(self.cart)

    def __getitem__(self, key):
        if not isinstance(key, int):
            raise TypeError('Индекс должен быть целым числом')
        if key < 0 or key >= len(self.cart):
            raise IndexError('Неверный индекс')
        return self.cart[key]

    def __iter__(self):
        print('Вызов метода __iter__()')
        yield from self.cart


order = Order(['банан', 'яблоко', 'лимон'], 'Кемаль')

print('арбуз' in order)
print('лимон' in order)
```

выводит:

```no-highlight
Вызов метода __iter__()
False
Вызов метода __iter__()
True
```

Метод `__iter__()` также может быть опущен, так как если у объекта есть длина и возможность обращаться к его элементам по индексам, то этого достаточно, чтобы проитерироваться по нему вручную. Таким образом, если в некотором классе определены методы `__len__()` и `__getitem__()`, то его экземпляры уже можно назвать последовательностью.​​​​