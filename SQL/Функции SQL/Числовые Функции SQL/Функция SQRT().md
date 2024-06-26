Функция `SQRT()` используется для вычисления квадратного корня. Она принимает в качестве аргумента число, извлекает из него квадратный корень и возвращает полученный результат.

Результатом приведенного ниже запроса:

```sql
SELECT SQRT(0),
       SQRT(9),
       SQRT(20);
```

является:

```no-highlight
+---------+---------+------------------+
| SQRT(0) | SQRT(9) | SQRT(20)         |
+---------+---------+------------------+
| 0.0     | 3.0     | 4.47213595499958 |
+---------+---------+------------------+
```

Если переданное в качестве аргумента число меньше нуля, функция `SQRT()` вернет значение `NULL`.

Результатом приведенного ниже запроса:

```sql
SELECT SQRT(-1),
       SQRT(-100);
```

является:

```no-highlight
+----------+------------+
| SQRT(-1) | SQRT(-100) |
+----------+------------+
| NULL     | NULL       |
+----------+------------+
```