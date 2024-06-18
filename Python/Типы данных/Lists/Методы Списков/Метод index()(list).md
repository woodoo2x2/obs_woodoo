

 
Метод `index()` возвращает индекс первого элемента, значение которого равняется переданному в метод значению. Таким образом, в метод передается один параметр:

1. `value`: значение, индекс которого требуется найти.

Если элемент в списке не найден, то во время выполнения происходит ошибка.

Следующий программный код:

```python
names = ['Gvido', 'Roman' , 'Timur']
position = names.index('Timur')
print(position)
```

выведет:

```no-highlight
2
```

Следующий программный код:

```python
names = ['Gvido', 'Roman' , 'Timur']
position = names.index('Anders')
print(position)
```

приводит к ошибке:

```no-highlight
ValueError: 'Anders' is not in list
```

Чтобы избежать таких ошибок, можно использовать метод `index()` вместе с оператором принадлежности `in`:

```python
names = ['Gvido', 'Roman' , 'Timur']
if 'Anders' in names:
    position = names.index('Anders')
    print(position)
else:
    print('Такого значения нет в списке')
```
