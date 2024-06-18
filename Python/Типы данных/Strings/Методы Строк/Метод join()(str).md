



Метод `join()` собирает строку из элементов списка, используя в качестве разделителя строку, к которой применяется метод.

 Следующий программный код:

```python
words = ['Python', 'is', 'the', 'most', 'powerful', 'language']
s = ' '.join(words)
print(s)
```

выведет: 

```no-highlight
Python is the most powerful language
```

![](https://ucarecdn.com/b32212f5-7b54-4d77-96d4-7f754c6f967e/)

Обратите внимание, все слова разделены одним пробелом, поскольку метод `join()` вызывался на строке, состоящей из одного символа пробела `' '`.

Рассмотрим еще пару примеров:

```python
words = ['Мы', 'учим', 'язык', 'Python']
print('*'.join(words))
print('-'.join(words))
print('?'.join(words))
print('!'.join(words))
print('*****'.join(words))
print('abc'.join(words))
print('123'.join(words))
```

Результатом выполнения такого кода будет:

```no-highlight
Мы*учим*язык*Python
Мы-учим-язык-Python
Мы?учим?язык?Python
Мы!учим!язык!Python
Мы*****учим*****язык*****Python
МыabcучимabcязыкabcPython
Мы123учим123язык123Python
```

