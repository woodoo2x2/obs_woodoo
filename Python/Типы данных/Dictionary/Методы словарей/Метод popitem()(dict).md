

Метод `popitem()` удаляет из словаря **последний добавленный элемент** и возвращает удаляемый элемент в виде кортежа `(ключ, значение)`.

Следующий программный код:

```python
info = {'name': 'Bob',
     'age': 25,
     'job': 'Dev'}

info['surname'] = 'Sinclar'

item = info.popitem()

print(item)
print(info)
```

выводит:

```no-highlight
('surname', 'Sinclar')
{'name': 'Bob', 'age': 25, 'job': 'Dev'}
```

 В версиях Python ниже 3.6 метод `popitem()` удалял случайный элемент.