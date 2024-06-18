 Переименование поля

Переименование поля таблицы синтаксически похоже на переименование самой таблицы и выполняется с помощью оператора `RENAME COLUMN`. В качестве примера его использования напишем запрос, который изменяет название поля `author` таблицы `Books` на `writer`.

После выполнения приведенного ниже запроса:

```sql
ALTER TABLE Books
RENAME COLUMN author TO writer;
```

таблица `Books` будет иметь вид:

```no-highlight
+----+------------------------------------------+-----------------+
| id | title                                    | writer          |
+----+------------------------------------------+-----------------+
| 1  | Fight Club                               | Chuck Palahniuk |
| 2  | The Green Mile                           | Stephen King    |
| 3  | The Lord of the Rings                    | J.R.R. Tolkien  |
| 4  | It                                       | Stephen King    |
| 5  | Harry Potter and the Prisoner of Azkaban | J.K. Rowling    |
+----+------------------------------------------+-----------------+
```