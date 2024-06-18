

Метод `replace(<old>, <new>)` возвращает копию `s` **со всеми** вхождениями подстроки `<old>`, замененными на `<new>`.

Результатом выполнения следующего кода:

```python
s = 'foo bar foo baz foo qux'
print(s.replace('foo', 'grault'))
```

будет:

```no-highlight
grault bar grault baz grault qux
```

Метод `replace()` может принимать опциональный третий аргумент `<count>`,  который определяет количество замен.

Результатом выполнения следующего кода:

```python
s = 'foo bar foo baz foo qux'
print(s.replace('foo', 'grault', 2))
```

будет:

```no-highlight
grault bar grault baz foo qux
```
