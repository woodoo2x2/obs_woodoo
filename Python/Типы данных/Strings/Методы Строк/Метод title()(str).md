

Метод `title()` возвращает копию строки `s`, в которой первый символ каждого слова переводится в верхний регистр.

Результатом выполнения следующего кода:

```python
s = 'the sun also rises'
print(s.title())
```

будет:

```no-highlight
The Sun Also Rises
```

Этот метод использует довольно простой алгоритм: он не пытается различить важные и неважные слова и не обрабатывает аббревиатуры и апострофы. Результатом выполнения следующего кода:

```python
s = "what's happened to ted's IBM stock?"
print(s.title())
```

будет:

```no-highlight
What'S Happened To Ted'S Ibm Stock?
```
