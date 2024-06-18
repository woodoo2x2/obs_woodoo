

Доступ к использованным в функции аннотациям можно получить через атрибут `__annotations__`, в котором аннотации представлены в виде словаря, где ключами являются названия параметров, а значениями – их типы. При этом, возвращаемое функцией значение хранится в записи с ключом `return`.

Приведенный ниже код:

```python
def avg(num1: int, num2: int, num3: int) -> float:
    return (num1 + num2 + num3) / 3

print(avg.__annotations__)
```

выводит:

```no-highlight
{'num1': <class 'int'>, 'num2': <class 'int'>, 'num3': <class 'int'>, 'return': <class 'float'>}
```