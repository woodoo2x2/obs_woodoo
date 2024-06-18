

Сравнение `None` с любым объектом, отличным от `None`, дает значение `False`.

Следующий программный код:

```python
print(None == None)
```

выведет:

```no-highlight
True
```

Следующий программный код:

```python
print(None == 17)
print(None == 3.14)
print(None == True)
print(None == [1, 2, 3])
print(None == 'Beegeek')
```

выведет:

```no-highlight
False
False
False
False
False
```

Важно понимать, что следующий программный код:

```python
print(None == 0)
print(None == False)
print(None == '')
```

выведет:

```no-highlight
False
False
False
```

  Значение `None` **не отождествляется** со значениями `0`, `False`, `''`.

Сравнивать `None` с другими типами данных можно только на равенство.

Следующий программный код:

```python
print(None > 0)
print(None <= False)
```

приводит к ошибке:

```no-highlight
TypeError: '>' not supported between instances of 'NoneType' and 'int' ('bool')
```
