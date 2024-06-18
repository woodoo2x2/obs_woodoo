

Логический оператор `NOT` служит только одной цели — отрицать условие, следующее за ним. Например, с помощью данного оператора мы можем извлечь данные о тех песнях, исполнителем которых не является группа `The Sounds`.

Результатом приведенного ниже запроса:

```sql
SELECT trackname, artist
FROM Songs
WHERE NOT artist = 'The Sounds';
```

является:

```no-highlight
+----------------------+-----------+
| trackname            | artist    |
+----------------------+-----------+
| Crazy on You         | Heart     |
| Running up That Hill | Kate Bush |
| Spent the Day in Bed | Morrissey |
+----------------------+-----------+
```

Логический оператор `NOT` отрицает следующее за ним условие, поэтому возвращаются не те записи, которые в поле `artist` содержат значение `The Sounds`, а все остальные.

Конечно, в данном случае аналогичный по функционалу запрос можно составить и без оператора `NOT`, воспользовавшись лишь оператором сравнения `!=`.

Результатом приведенного ниже запроса:

```sql
SELECT trackname, artist
FROM Songs
WHERE artist != 'The Sounds';
```

является:

```no-highlight
+----------------------+-----------+
| trackname            | artist    |
+----------------------+-----------+
| Crazy on You         | Heart     |
| Running up That Hill | Kate Bush |
| Spent the Day in Bed | Morrissey |
+----------------------+-----------+
```

Однако в более сложных условиях можно воспользоваться оператором `NOT` в связке с оператором `IN`, что окажется предпочтительнее оператора `!=`.

Результатом приведенного ниже запроса:

```sql
SELECT trackname, artist
FROM Songs
WHERE NOT artist IN ('Heart', 'Kate Bush', 'Morrissey');
```

является:

```no-highlight
+-----------+------------+
| trackname | artist     |
+-----------+------------+
| My Lover  | The Sounds |
| Thrill    | The Sounds |
+-----------+------------+
```

В этом запросе извлекаются данные о тех песнях, исполнителями которых не являются `Heart, Kate Bush` и `Morrissey`. Аналогичный запрос с использованием оператора сравнения `!=` выглядел бы следующим образом:

```sql
SELECT trackname, artist
FROM Songs
WHERE artist != 'Heart' AND artist != 'Kate Bush' AND artist != 'Morrissey';
```

Логический оператор `NOT` отрицает только то условие, перед которым указан.

Результатом приведенного ниже запроса:

```sql
SELECT id, artist
FROM Songs
WHERE NOT id = 1 OR id = 2;
```

 является:

```no-highlight
+----+------------+
| id | artist     |
+----+------------+
| 2  | The Sounds |
| 3  | Kate Bush  |
| 4  | The Sounds |
| 5  | Morrissey  |
+----+------------+
```

В запросе выше указано два условия, но оператор `NOT` отрицает только первое из них. Для того, чтобы он отрицал все условия, необходимо объединить их вместе с помощью круглых скобок.

Результатом приведенного ниже запроса:

```sql
SELECT id, artist
FROM Songs
WHERE NOT (id = 1 OR id = 2);
```

 является:

```no-highlight
+----+------------+
| id | artist     |
+----+------------+
| 3  | Kate Bush  |
| 4  | The Sounds |
| 5  | Morrissey  |
+----+------------+
```