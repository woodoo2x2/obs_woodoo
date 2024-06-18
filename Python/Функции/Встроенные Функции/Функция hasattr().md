

Функция `hasattr()` принимает два аргумента:

- `obj` — объект
- `name` — имя атрибута

Функция возвращает `True`, если объект `obj` имеет атрибут `name`, или `False` в противном случае.

Приведенный ниже код: 

```python
class Cat:
    night_vision = True
    paws_count = 4


cat = Cat()

cat.name = 'Кемаль'

print(hasattr(cat, 'name'))
print(hasattr(cat, 'age'))
print(hasattr(cat, 'night_vision'))
```

выводит:

```no-highlight
True
False
True
```