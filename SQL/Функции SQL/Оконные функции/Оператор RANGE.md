Определить границы окна также можно с помощью оператора `RANGE`, синтаксис использования которого имеет следующий вид:

```css
RANGE BETWEEN <начальная граничная точка> AND <конечная граничная точка>
```

Как можно заметить, применяется оператор `RANGE` практически так же, как и оператор `ROWS`. Более того, для указания граничных точек используются те же ключевые слова `CURRENT ROW, PRECEDING` и `FOLLOWING`.

Несмотря на схожесть синтаксисов, принципы работы операторов `RANGE` и `ROWS` значительно отличаются. Оператор `RANGE`, в отличие от `ROWS`, ориентируется не на расположение записей (например, предыдущая или следующая), а на их значение в том поле, **по которому было упорядочено окно**.

Предположим, мы работаем с окном (не разбитым на секции), которое имеет следующий вид:

```no-highlight
+----+-----------------+-------------+--------+
| id | full_name       | department  | salary |
+----+-----------------+-------------+--------+
| 3  | Larry Page      | Engineering | 7000   |
| 4  | Eric Schmidt    | Marketing   | 8000   |
| 1  | Sergey Brin     | Engineering | 10000  |
| 2  | John Doerr      | Sales       | 10000  |
| 5  | Sundar Pichai   | Sales       | 11000  |
+----+-----------------+-------------+--------+
```

Также допустим, что записи в данном окне упорядочены по полю `salary`, а границы определены с помощью оператора `RANGE` следующим образом:

```css
RANGE BETWEEN 2000 PRECEDING AND 2000 FOLLOWING
```

Тогда применяемая к окну оконная функция во время работы, например, со второй записью (`id = 4`) будет выполнять вычисления лишь в рамках следующей части окна:

```no-highlight
+----+-----------------+-------------+--------+
| id | full_name       | department  | salary |
+----+-----------------+-------------+--------+
| 3  | Larry Page      | Engineering | 7000   | ┐  <-- значение поля salary отличается от 8000 на 1000
| 4  | Eric Schmidt    | Marketing   | 8000   | │  <-- текущая запись
| 1  | Sergey Brin     | Engineering | 10000  | │  <-- значение поля salary отличается от 8000 на 2000
| 2  | John Doerr      | Sales       | 10000  | ┘  <-- значение поля salary отличается от 8000 на 2000
| 5  | Sundar Pichai   | Sales       | 11000  |    <-- значение поля salary отличается от 8000 на 3000
+----+-----------------+-------------+--------+
```

То есть часть `2000 PRECEDING` в данном контексте говорит о том, что в ограниченную часть окна должны попасть те записи, которые располагаются в окне до текущей записи и содержат в поле `salary` значение, отличающееся от `8000` (значение текущей записи в поле `salary`) не более чем на `2000`. Часть `2000 FOLLOWING`, в свою очередь, сообщает то, что в ограниченную часть окна должны попасть те записи, которые располагаются в окне после текущей записи и также содержат в поле `salary` значение, отличающееся от `8000` не более чем на `2000`.

Таким образом, оператор `RANGE` совместно с ключевыми словами `PRECEDING` и `FOLLOWING` позволяет определять не столько границы, сколько диапазоны. Если некоторое окно упорядочено по условному полю `field`, а текущая запись в поле `field` содержит значение `x`, тогда определение границ следующим образом:

```css
RANGE BETWEEN a PRECEDING AND b FOLLOWING
```

будет говорить о том, что в ограниченную часть окна должны быть включены те записи, которые в поле `field` содержат значение, входящее в диапазон `[x - a; x + b]`.

При использовании оператора `RANGE` связка ключевых слов `CURRENT ROW` обозначает не текущую запись, а набор записей, которые в поле упорядочивания содержат то же значение, что и текущая запись. Другими словами, `CURRENT ROW` в контексте оператора `RANGE` можно трактовать как `0 PRECEDING` или `0 FOLLOWING`.

Результатом приведенного ниже запроса:

```sql
SELECT full_name, salary,
       COUNT(*) OVER (ORDER BY salary RANGE BETWEEN CURRENT ROW AND CURRENT ROW) AS result
FROM Employees
```

является:

```no-highlight
+-----------------+--------+--------+
| full_name       | salary | result |
+-----------------+--------+--------+
| Larry Page      | 7000   | 2      |
| John Smith      | 7000   | 2      |
| Eric Schmidt    | 8000   | 2      |
| Susan Wojcicki  | 8000   | 2      |
| Marissa Mayer   | 9000   | 2      |
| Sheryl Sandberg | 9000   | 2      |
| Sergey Brin     | 10000  | 3      |
| John Doerr      | 10000  | 3      |
| Alice Johnson   | 10000  | 3      |
| Sundar Pichai   | 11000  | 1      |
+-----------------+--------+--------+
```

 Окно, границы которого определяются с помощью оператора `RANGE`, должно быть упорядочено **только по одному полю**.

 Поле, которое используется оператором `RANGE` для определения границ, должно содержать только числа, даты, временные значения или значения даты и времени.

Окно всегда имеет границы, определенные по умолчанию, которые зависят от того, упорядочено окно или нет. Если окно не упорядочено, границы определяются следующим образом:

```css
RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
```

если упорядочено — следующим образом:

```css
RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```

Результатом приведенного ниже запроса:

```sql
SELECT id, full_name, salary,
       AVG(salary) OVER () AS not_sorted,
       AVG(salary) OVER (ORDER BY salary) AS sorted,
       AVG(salary) OVER (ORDER BY salary RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS sorted_and_limited
FROM Employees;
```

является:

```no-highlight
+----+-----------------+--------+------------+-----------+--------------------+
| id | full_name       | salary | not_sorted | sorted    | sorted_and_limited |
+----+-----------------+--------+------------+-----------+--------------------+
| 3  | Larry Page      | 7000   | 8900.0000  | 7000.0000 | 7000.0000          |
| 8  | John Smith      | 7000   | 8900.0000  | 7000.0000 | 7000.0000          |
| 4  | Eric Schmidt    | 8000   | 8900.0000  | 7500.0000 | 7500.0000          |
| 7  | Susan Wojcicki  | 8000   | 8900.0000  | 7500.0000 | 7500.0000          |
| 6  | Marissa Mayer   | 9000   | 8900.0000  | 8000.0000 | 8000.0000          |
| 9  | Sheryl Sandberg | 9000   | 8900.0000  | 8000.0000 | 8000.0000          |
| 1  | Sergey Brin     | 10000  | 8900.0000  | 8666.6667 | 8666.6667          |
| 2  | John Doerr      | 10000  | 8900.0000  | 8666.6667 | 8666.6667          |
| 10 | Alice Johnson   | 10000  | 8900.0000  | 8666.6667 | 8666.6667          |
| 5  | Sundar Pichai   | 11000  | 8900.0000  | 8900.0000 | 8900.0000          |
+----+-----------------+--------+------------+-----------+--------------------+
```