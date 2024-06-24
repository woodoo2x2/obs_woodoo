

Функция `FIRST_VALUE()` используется для получения значения, которое содержится в определенном поле первой записи окна. В качестве примера ее использования напишем запрос, который зарплате каждого сотрудника ставит в соответствие зарплату самого первого сотрудника (сотрудника с наименьшим идентификатором).

Результатом приведенного ниже запроса:

```sql
SELECT id, full_name, salary,
       FIRST_VALUE(salary) OVER (ORDER BY id) AS first_employee_salary
FROM Employees;
```

является:

```no-highlight
+----+-----------------+--------+-----------------------+
| id | full_name       | salary | first_employee_salary |
+----+-----------------+--------+-----------------------+
| 1  | Sergey Brin     | 10000  | 10000                 |
| 2  | John Doerr      | 10000  | 10000                 |
| 3  | Larry Page      | 7000   | 10000                 |
| 4  | Eric Schmidt    | 8000   | 10000                 |
| 5  | Sundar Pichai   | 11000  | 10000                 |
| 6  | Marissa Mayer   | 9000   | 10000                 |
| 7  | Susan Wojcicki  | 8000   | 10000                 |
| 8  | John Smith      | 7000   | 10000                 |
| 9  | Sheryl Sandberg | 9000   | 10000                 |
| 10 | Alice Johnson   | 12000  | 10000                 |
+----+-----------------+--------+-----------------------+
```

В примере выше функция `FIRST_VALUE()` применяется к окну, упорядоченному по полю `id`, и для каждой записи оконная функция возвращает значение, которое хранится в поле `salary` первой записи окна, — `10000`.

Дополнив запрос выше разбиением окна на секции, можно поставить в соответствие зарплату самого первого сотрудника не среди всех сотрудников, а лишь в рамках определенного отдела.

Результатом приведенного ниже запроса:

```sql
SELECT id, full_name, department, salary,
       FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY id) AS first_employee_salary_in_department
FROM Employees;
```

является:

```no-highlight
+----+-----------------+-------------+--------+-------------------------------------+
| id | full_name       | department  | salary | first_employee_salary_in_department |
+----+-----------------+-------------+--------+-------------------------------------+
| 1  | Sergey Brin     | Engineering | 10000  | 10000                               |
| 3  | Larry Page      | Engineering | 7000   | 10000                               |
| 7  | Susan Wojcicki  | Engineering | 8000   | 10000                               |
| 8  | John Smith      | Engineering | 7000   | 10000                               |
| 10 | Alice Johnson   | Engineering | 12000  | 10000                               |
| 4  | Eric Schmidt    | Marketing   | 8000   | 8000                                |
| 6  | Marissa Mayer   | Marketing   | 9000   | 8000                                |
| 9  | Sheryl Sandberg | Marketing   | 9000   | 8000                                |
| 2  | John Doerr      | Sales       | 10000  | 10000                               |
| 5  | Sundar Pichai   | Sales       | 11000  | 10000                               |
+----+-----------------+-------------+--------+-------------------------------------+
```