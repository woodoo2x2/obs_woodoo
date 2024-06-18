Мы можем сравнивать простые (тип [[set (Множества)]]) и замороженные множества (тип [[Frozenset]]).

Приведенный ниже код:

```python
myset1 = set('qwerty')
myset2 = frozenset('qwerty')

print(myset1 == myset2)
```

выведет:

```no-highlight
True
```