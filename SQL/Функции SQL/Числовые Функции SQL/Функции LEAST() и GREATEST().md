

Функция `LEAST()` используется для поиска минимального значения. Она принимает переменное количество аргументов (не меньше двух) и возвращает наименьший из них.

Результатом приведенного ниже запроса:

```sql
SELECT LEAST(2, 1, 3, 5, 4);
```

является:

```no-highlight
+----------------------+
| LEAST(2, 1, 3, 5, 4) |
+----------------------+
| 1                    |
+----------------------+
```

Если поиск минимального значения происходит среди действительных (типы `FLOAT` и `DOUBLE`) и целых чисел, функция `LEAST()` сначала преобразует все числа в действительные.

Результатом приведенного ниже запроса:

```sql
SELECT LEAST(2, 1, 3.0, 5, 4);
```

является:

```no-highlight
+------------------------+
| LEAST(2, 1, 3.0, 5, 4) |
+------------------------+
| 1.0                    |
+------------------------+
```

Если хотя бы один из аргументов, переданных в функцию `LEAST()`, является строкой, функция перед поиском минимального значения сначала преобразует все аргументы в строки.

Результатом приведенного ниже запроса:

```sql
SELECT LEAST(100, '11');
```

является:

```no-highlight
+------------------+
| LEAST(100, '11') |
+------------------+
| 100              |
+------------------+
```

Если хотя бы один из аргументов, переданных в функцию `LEAST()`, равняется `NULL`, возвращаемым значением функции также будет `NULL`.

Результатом приведенного ниже запроса:

```sql
SELECT LEAST(2, 1, NULL, 5, 4);
```

является:

```no-highlight
+-------------------------+
| LEAST(2, 1, NULL, 5, 4) |
+-------------------------+
| NULL                    |
+-------------------------+
```

Похожим образом себя ведет функция `GREATEST()` за тем исключением, что она выполняет поиск наибольшего значения, а не наименьшего.

Результатом приведенного ниже запроса:

```sql
SELECT GREATEST(2, 1, 3, 5, 4);
```

является:

```no-highlight
+-------------------------+
| GREATEST(2, 1, 3, 5, 4) |
+-------------------------+
| 5                       |
+-------------------------+
```