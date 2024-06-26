Функция `REPLACE()` используется для замены подстроки в строке. Она принимает три аргумента в следующем порядке:

- `str` — исходная строка
- `from_str` — заменяемая подстрока
- `to_str` — заменяющая подстрока

Функция заменяет все вхождения подстроки `from_str` в строке `str` на подстроку `to_str` и возвращает полученный результат.

Результатом приведенного ниже запроса:

```sql
SELECT REPLACE('beegeek', 'e', 'i'),
       REPLACE('beegeek', 'geek', 'g'),
       REPLACE('beegeek', 'geek', 'Geek');
```

является:

```no-highlight
+------------------------------+---------------------------------+------------------------------------+
| REPLACE('beegeek', 'e', 'i') | REPLACE('beegeek', 'geek', 'g') | REPLACE('beegeek', 'geek', 'Geek') |
+------------------------------+---------------------------------+------------------------------------+
| biigiik                      | beeg                            | beeGeek                            |
+------------------------------+---------------------------------+------------------------------------+
```

Если заменяемой подстроки в строке нет, функция `REPLACE()` вернет строку в исходном виде.

Результатом приведенного ниже запроса:

```sql
SELECT REPLACE('beegeek', 'a', 'i'),
       REPLACE('beegeek', 'keeg', 'Keeg');
```

является:

```no-highlight
+------------------------------+------------------------------------+
| REPLACE('beegeek', 'a', 'i') | REPLACE('beegeek', 'keeg', 'Keeg') |
+------------------------------+------------------------------------+
| beegeek                      | beegeek                            |
+------------------------------+------------------------------------+
```

В отличие от функции `LOCATE()`, функция `REPLACE()` выполняет замену с учетом регистра.

Результатом приведенного ниже запроса:

```sql
SELECT REPLACE('beegeek', 'B', 'BBB'),
       REPLACE('beegeek', 'b', 'bbb');
```

является:

```no-highlight
+--------------------------------+--------------------------------+
| REPLACE('beegeek', 'B', 'BBB') | REPLACE('beegeek', 'b', 'bbb') |
+--------------------------------+--------------------------------+
| beegeek                        | bbbeegeek                      |
+--------------------------------+--------------------------------+
```