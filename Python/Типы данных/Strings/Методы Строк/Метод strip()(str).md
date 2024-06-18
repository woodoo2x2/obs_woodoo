
Метод `strip()` возвращает копию строки `s` у которой удалены все пробелы стоящие **в начале и конце** строки.

Результатом выполнения следующего кода:

```python
s = '     foo bar foo baz foo qux      '
print(s.strip())
```

будет:

```no-highlight
foo bar foo baz foo qux
```