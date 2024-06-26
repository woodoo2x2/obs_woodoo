 

Функция `delattr()` принимает два аргумента:

- `obj` — объект
- `name` — имя атрибута

Функция удаляет атрибут `name` у объекта `obj`. Если объект не имеет атрибута `name`, возбуждается исключение `AttributeError`.

Приведенный ниже код:

```python
class Cat:
    pass


cat = Cat()

cat.name = 'Кемаль'
cat.age = 1

print(cat.__dict__)

delattr(cat, 'name')
delattr(cat, 'age')

print(cat.__dict__)
```

выводит:

```no-highlight
{'name': 'Кемаль', 'age': 1}
{}
```

При попытке удаления атрибута класса через его экземпляр с помощью функции `delattr()` возникнет ошибка, поскольку функция `delattr()` работает с конкретным объектом.