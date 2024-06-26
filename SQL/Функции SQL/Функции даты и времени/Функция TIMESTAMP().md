Функция `TIMESTAMP()` используется для объединения даты и временного значения. Она принимает два аргумента в следующем порядке:

- `date` — дата
- `time` — временное значение

Функция объединяет дату `date` и время `time` и возвращает полученный результат в виде единого объекта.

Результатом приведенного ниже запроса:

```sql
SELECT TIMESTAMP('2023-10-20', '08:00'),
       TIMESTAMP('2023-10-20', '28:00');
```

является:

```no-highlight
+----------------------------------+----------------------------------+
| TIMESTAMP('2023-10-20', '08:00') | TIMESTAMP('2023-10-20', '28:00') |
+----------------------------------+----------------------------------+
| 2023-10-20 08:00:00              | 2023-10-21 04:00:00              |
+----------------------------------+----------------------------------+
```

Обратите внимание, что функция `TIMESTAMP()` автоматически конвертирует каждые `24` часа в `1` день.

Временное значение может не указываться, в таком случае оно будет принято равным `00:00:00`.

Результатом приведенного ниже запроса:

```sql
SELECT TIMESTAMP('2023-10-20');
```

является:

```no-highlight
+-------------------------+
| TIMESTAMP('2023-10-20') |
+-------------------------+
| 2023-10-20 00:00:00     |
+-------------------------+
```