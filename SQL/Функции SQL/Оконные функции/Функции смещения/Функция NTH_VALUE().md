Функция `NTH_VALUE()` используется для получения значения, которое содержится в определенном поле `n`-ой записи окна (начиная с `1`). В качестве примера ее использования напишем запрос, который зарплате каждого сотрудника ставит в соответствие зарплаты второго и третьего сотрудников.

Результатом приведенного ниже запроса:

```sql
SELECT id, full_name, salary,
       NTH_VALUE(salary, 2) OVER id_asc AS second_employee_salary,
       NTH_VALUE(salary, 3) OVER id_asc AS third_employee_salary
FROM Employees
WINDOW id_asc AS (ORDER BY id ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING);
```

является:

```no-highlight
+----+-----------------+--------+------------------------+-----------------------+
| id | full_name       | salary | second_employee_salary | third_employee_salary |
+----+-----------------+--------+------------------------+-----------------------+
| 1  | Sergey Brin     | 10000  | 10000                  | 7000                  |
| 2  | John Doerr      | 10000  | 10000                  | 7000                  |
| 3  | Larry Page      | 7000   | 10000                  | 7000                  |
| 4  | Eric Schmidt    | 8000   | 10000                  | 7000                  |
| 5  | Sundar Pichai   | 11000  | 10000                  | 7000                  |
| 6  | Marissa Mayer   | 9000   | 10000                  | 7000                  |
| 7  | Susan Wojcicki  | 8000   | 10000                  | 7000                  |
| 8  | John Smith      | 7000   | 10000                  | 7000                  |
| 9  | Sheryl Sandberg | 9000   | 10000                  | 7000                  |
| 10 | Alice Johnson   | 10000  | 10000                  | 7000                  |
+----+-----------------+--------+------------------------+-----------------------+
```

Здесь оба вызова функции `NTH_VALUE()` выполняются относительно окна, упорядоченного по полю `id`, границы которого определены следующим образом: от самой первой записи до самой последней. Для каждой записи вызов оконной функции с аргументом `2` возвращает значение, которое хранится в поле `salary` второй по счету записи окна — `10000`, а вызов с аргументом `3` — значение, которое хранится в поле `salary` третьей по счету записи окна — `7000`.

Во время использования функции `NTH_VALUE()`, как и в случае с функцией `LAST_VALUE()`, важно следить за тем, как определены границы окна, в противном случае оконная функция может не дать нужного результата.

Функция `NTH_VALUE()` возвращает значение `NULL`, если пытается получить значение записи, выходящей за рамки окна.

Результатом приведенного ниже запроса:

```sql
SELECT id, full_name, salary,
       NTH_VALUE(salary, 11) OVER (ORDER BY id
                                   ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_employee_salary
FROM Employees;
```

является:

```no-highlight
+----+-----------------+--------+----------------------+
| id | full_name       | salary | last_employee_salary |
+----+-----------------+--------+----------------------+
| 1  | Sergey Brin     | 10000  | NULL                 |
| 2  | John Doerr      | 10000  | NULL                 |
| 3  | Larry Page      | 7000   | NULL                 |
| 4  | Eric Schmidt    | 8000   | NULL                 |
| 5  | Sundar Pichai   | 11000  | NULL                 |
| 6  | Marissa Mayer   | 9000   | NULL                 |
| 7  | Susan Wojcicki  | 8000   | NULL                 |
| 8  | John Smith      | 7000   | NULL                 |
| 9  | Sheryl Sandberg | 9000   | NULL                 |
| 10 | Alice Johnson   | 12000  | NULL                 |
+----+-----------------+--------+----------------------+
```

В данном примере оконная функция пробует получить значение поля `salary` одиннадцатой записи окна, однако окно включает лишь десять записей.