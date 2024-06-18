

Метод `startswith(<suffix>, <start>, <end>)` определяет, **начинается** ли исходная строка `s` подстрокой `<suffix>`. Если исходная строка начинается с подстроки <suffix>, метод возвращает значение True, а если нет, то  False.

Результатом выполнения следующего кода:

```python
s = 'foobar'
print(s.startswith('foo'))
print(s.startswith('baz'))
```

будет:

```no-highlight
True
False
```