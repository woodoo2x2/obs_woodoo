Метод `copy()` создает поверхностную копию списка.

Следующий программный код:

```python
names = ['Gvido', 'Roman' , 'Timur']
names_copy = names.copy()              # создаем поверхностную копию списка names

print(names)
print(names_copy)
```

выведет:

```python
['Gvido', 'Roman', 'Timur']
['Gvido', 'Roman', 'Timur']
```

Аналогичного результата можно достичь с помощью срезов или функции `list()`:

```python
names = ['Gvido', 'Roman' , 'Timur']
names_copy1 = list(names)             # создаем поверхностную копию с помощью функции list()
names_copy2 = names[:]                # создаем поверхностную копию с помощью среза от начала до конца
```

Подробнее про поверхностные копии мы поговорим в курсе для продвинутых.