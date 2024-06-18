

В SQL перекрестное соединение выполняется с помощью оператора `CROSS JOIN`. Сначала указывается первая таблица, затем оператор `CROSS JOIN`, а после вторая таблица.

Общий синтаксис перекрестного соединения имеет следующий вид:

```css
<первая таблица> CROSS JOIN <вторая таблица>
```

В качестве примера выполним перекрестное соединение таблиц `Students` и `Subjects`.

Результатом приведенного ниже запроса:

```
SELECT *
FROM Subjects CROSS JOIN Students;
```

является:

```no-highlight
+----+--------------+----+-----------+
| id | student      | id | name      |
+----+--------------+----+-----------+
| 1  | Peter Parker | 3  | Physics   |
| 1  | Peter Parker | 2  | Chemistry |
| 1  | Peter Parker | 1  | Math      |
| 2  | Mary Jane    | 3  | Physics   |
| 2  | Mary Jane    | 2  | Chemistry |
| 2  | Mary Jane    | 1  | Math      |
| 3  | Gwen Stacy   | 3  | Physics   |
| 3  | Gwen Stacy   | 2  | Chemistry |
| 3  | Gwen Stacy   | 1  | Math      |
| 4  | Harry Osborn | 3  | Physics   |
| 4  | Harry Osborn | 2  | Chemistry |
| 4  | Harry Osborn | 1  | Math      |
+----+--------------+----+-----------+
```