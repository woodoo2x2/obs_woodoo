

Метод `swapcase()` возвращает копию строки `s`, в которой все символы, имеющие верхний регистр, преобразуются в символы нижнего регистра и наоборот.

 Результатом выполнения следующего кода:

```python
s = 'FOO Bar 123 baz qUX'
print(s.swapcase())
```

будет:

```no-highlight
foo bAR 123 BAZ Qux
```