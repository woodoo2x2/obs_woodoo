

Чтобы отфильтровать данные по нескольким полям, необходимо воспользоваться оператором `AND`. Он используется для извлечения только тех записей, которые удовлетворяют **всем** указанным условиям.

Результатом приведенного ниже запроса:

```sql
SELECT trackname, artist, streams, release_date
FROM Songs
WHERE streams > 50000 AND release_date >= '2000-01-01';
```

является:

```no-highlight
+----------------------+------------+---------+--------------+
| trackname            | artist     | streams | release_date |
+----------------------+------------+---------+--------------+
| My Lover             | The Sounds | 99488   | 2009-05-31   |
| Spent the Day in Bed | Morrissey  | 174994  | 2017-09-19   |
+----------------------+------------+---------+--------------+
```

Посредством запроса выше извлекаются данные о тех песнях, количество прослушиваний которых превышает `50000` и которые были выпущены в `2000` году или позднее. После оператора `WHERE` содержатся два условия, а оператор `AND` используется для их объединения. Оператор `AND` говорит о том, что должны быть возвращены только те записи, которые удовлетворяют всем перечисленным условиям. Если количество прослушиваний песни превышает `50000`, но она была выпущена до `2000` года, то она не попадет в результат запроса. Аналогично если песня была выпущена в `2000` году или позднее, но не имеет более `50000` прослушиваний, то она также не попадет в результат запроса.

Количество условий после оператора `WHERE` может быть больше двух, в таком случае каждое из них должно отделяться отдельным оператором `AND`.