

Метод `lstrip()` возвращает копию строки `s` у которой удалены все пробелы стоящие **в начале** строки.

Результатом выполнения следующего кода:

```python
s = '     foo bar foo baz foo qux      '
print(s.lstrip())
```

будет:

```no-highlight
foo bar foo baz foo qux⎵ ⎵ ⎵ ⎵ ⎵ ⎵
```
