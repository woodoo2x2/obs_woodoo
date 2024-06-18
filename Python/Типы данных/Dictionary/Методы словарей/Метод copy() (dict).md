

Метод `copy()` создает **поверхностную** копию словаря.

Следующий программный код:

```python
info = {'name': 'Bob',
        'age': 25,
        'job': 'Dev'}

info_copy = info.copy()

print(info_copy)
```

выведет:

```no-highlight
{'name': 'Bob', 'age': 25, 'job': 'Dev'}
```

Не стоит путать копирование словаря (метод `copy()`) и присвоение новой переменной ссылки на старый словарь.

Следующий программный код:

```python
info = {'name': 'Bob',
        'age': 25,
        'job': 'Dev'}

new_info = info
new_info['name'] = 'Tim'

print(info)
```

выводит:

```no-highlight
{'name': 'Tim', 'age': 25, 'job': 'Dev'}
```

Оператор присваивания (`=`) не копирует словарь, а лишь присваивает ссылку на старый словарь новой переменной.

![](https://ucarecdn.com/95fb3a80-5be7-4b57-ac79-713a933d596d/)

Таким образом, когда мы изменяем словарь `new_info`, меняется и словарь `info`. Если необходимо изменить один словарь, не изменяя второй, используют метод `copy()`.

Следующий программный код:​

```python
info = {'name': 'Bob',
        'age': 25,
        'job': 'Dev'}

new_info = info.copy()
new_info['name'] = 'Tim'

print(info)
print(new_info)
```

выводит:

```no-highlight
{'name': 'Bob', 'age': 25, 'job': 'Dev'}
{'name': 'Tim', 'age': 25, 'job': 'Dev'}
```