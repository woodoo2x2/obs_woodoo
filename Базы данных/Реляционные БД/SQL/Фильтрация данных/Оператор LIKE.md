Важной особенностью оператора `LIKE` является то, что при поиске с его помощью строк, соответствующих шаблону, не учитывается регистр символов, используемых в этом шаблоне. Например, один из рассмотренных нами шаблонов поиска `%You%` мы могли записать в виде `%you%` или `%YOU%`, и результат остался бы прежним.

Результатом приведенного ниже запроса:

```sql
SELECT trackname, artist
FROM Songs
WHERE trackname LIKE '%you%';
```

является:

```no-highlight
+--------------------------------------+------------+
| trackname                            | artist     |
+--------------------------------------+------------+
| You Just Haven't Earned It Yet, Baby | The Smiths |
| Crazy on You                         | Heart      |
| Wish You Were Here                   | The Sounds |
| Let Me Kiss You                      | Morrissey  |
+--------------------------------------+------------+
```

Безусловно, такое поведение удобно, однако не всегда нужно. Поэтому в случаях, когда регистр символов важен, используется оператор `LIKE BINARY`, который работает аналогично оператору `LIKE`, но учитывает регистр символов, используемых в шаблоне поиска.

Результатом приведенного ниже запроса:

```sql
SELECT trackname, artist
FROM Songs
WHERE trackname LIKE BINARY '%You%';
```

является:

```no-highlight
+--------------------------------------+------------+
| trackname                            | artist     |
+--------------------------------------+------------+
| You Just Haven't Earned It Yet, Baby | The Smiths |
| Crazy on You                         | Heart      |
| Wish You Were Here                   | The Sounds |
| Let Me Kiss You                      | Morrissey  |
+--------------------------------------+------------+
```

При этом результат приведенного ниже запроса пуст:

```sql
SELECT trackname, artist
FROM Songs
WHERE trackname LIKE BINARY '%you%';
```