

Функция `setattr()` принимает три аргумента:

- `obj` — объект
- `name` — имя атрибута
- `value` — значение атрибута

Функция устанавливает объекту `obj` атрибут `name` со значением `value`. Если объект `obj` уже имеет атрибут `name`, его значение перезаписывается.

Приведенный ниже код: 

```python
class Cat:
    pass


cat = Cat()

cat.name = 'Кемаль'

setattr(cat, 'age', 1)
setattr(cat, 'name', 'Роджер')

print(cat.age)
print(cat.name)
```

выводит:

```no-highlight
1
Роджер
```