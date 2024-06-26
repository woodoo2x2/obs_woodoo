## Временные интервалы

В SQL для работы с датой и временем часто используются временные интервалы. Они представляют собой некоторый промежуток времени, выраженный в тех или иных единицах измерения (день, год, минута), и используются для выполнения арифметических операций с датой и временем.

Для записи временных интервалов применяется следующий синтаксис:

```css
INTERVAL <величина интервала> <единица измерения>
```

Единица измерения во временном интервале определяется ключевым словом. Например, день — это `DAY`, год — это `YEAR`, год и месяц — это `YEAR_MONTH`. Формат величины интервала напрямую зависит от единицы измерения. Для одиночных единиц измерения, таких как день или год, форматом величины является целое число. 

К примеру, интервал в `1` год определяется следующим образом:

```css
INTERVAL 1 YEAR
```

Для составных единиц измерения, таких как год и месяц, форматом величины является строка определенного вида, в которой перечислены все необходимые значения компонентов.

К примеру, интервал в `10` лет и `2` месяца определяется следующим образом:

```css
INTERVAL '10-2' YEAR_MONTH
```

### Поддерживаемые единицы измерения

SQL поддерживает большое количество единиц измерения. Для удобства мы разбили их на две таблицы, в одной из которых представлены одиночные единицы измерения, в другой — составные.

Итак, первая таблица включает все поддерживаемые одиночные единицы измерения:

|   |   |
|---|---|
|**Единица измерения**|**Ключевое слово**|
|микросекунда|`MICROSECOND`|
|секунда|`SECOND`|
|минута|`MINUTE`|
|час|`HOUR`|
|день|`DAY`|
|неделя|`WEEK`|
|месяц|`MONTH`|
|квартал|`QUARTER`|
|год|`YEAR`|

Вторая таблица включает все поддерживаемые составные единицы измерения, а также формат величины для каждой из единиц:

|   |   |   |
|---|---|---|
|**Единица измерения**|**Ключевое слово**|**Формат величины**|
|секунда и микросекунда|`SECOND_MICROSECOND`|`секунды.микросекунды`|
|минута, секунда и микросекунда|`MINUTE_MICROSECOND`|`минуты:секунды.микросекунды`|
|минута и секунда|`MINUTE_SECOND`|`минуты:секунды`|
|час, минута, секунда и микросекунда|`HOUR_MICROSECOND`|`часы:минуты:секунды.микросекунды`|
|час, минута и секунда|`HOUR_SECOND`|`часы:минуты:секунды`|
|час и минута|`HOUR_MINUTE`|`часы:минуты`|
|день, час, минута, секунда и микросекунда|`DAY_MICROSECOND`|`дни часы:минуты:секунды.микросекунды`|
|день, час, минута и секунда|`DAY_SECOND`|`дни часы:минуты:секунды`|
|день, час и минута|`DAY_MINUTE`|`дни часы:минуты`|
|день и час|`DAY_HOUR`|`дни часы`|
|год и месяц|`YEAR_MONTH`|`годы-месяцы`|

### Примеры использования временных интервалов

Для большей наглядности рассмотрим несколько примеров использования временных интервалов.

**Пример 1.** Прибавим к дате `2023-01-01` интервал в `10` лет и `2` месяца.

Результатом приведенного ниже запроса:

```sql
SELECT '2023-01-01' + INTERVAL '10-2' YEAR_MONTH;
```

является:

```no-highlight
+-------------------------------------------+
| '2023-01-01' + INTERVAL '10-2' YEAR_MONTH |
+-------------------------------------------+
| 2033-03-01                                |
+-------------------------------------------+
```

**Пример 2.** Вычтем из даты и времени `2023-01-01 14:00:00` интервал в `1` час и `30` минут.

Результатом приведенного ниже запроса:

```sql
SELECT '2023-01-01 14:00:00' - INTERVAL '01:30' HOUR_MINUTE;
```

является:

```no-highlight
+------------------------------------------------------+
| '2023-01-01 14:00:00' - INTERVAL '01:30' HOUR_MINUTE |
+------------------------------------------------------+
| 2023-01-01 12:30:00                                  |
+------------------------------------------------------+
```

**Пример 3.** Прибавим к дате и времени `2023-01-01 14:00:00` интервал в `4` дня и `2` часа.

Результатом приведенного ниже запроса:

```sql
SELECT '2023-01-01 14:00:00' + INTERVAL '4 2' DAY_HOUR;
```

является:

```no-highlight
+-------------------------------------------------+
| '2023-01-01 14:00:00' + INTERVAL '4 2' DAY_HOUR |
+-------------------------------------------------+
| 2023-01-05 16:00:00                             |
+-------------------------------------------------+
```

**Пример 4.** Вычтем из даты `2023-01-01` интервал в `1` час.

Результатом приведенного ниже запроса:

```sql
SELECT '2023-01-01' - INTERVAL 1 HOUR;
```

является:

```no-highlight
+--------------------------------+
| '2023-01-01' - INTERVAL 1 HOUR |
+--------------------------------+
| 2022-12-31 23:00:00            |
+--------------------------------+
```

Обратите внимание, что из даты без времени допускается вычитать временное значение. Во время выполнения такой операции считается, что дата имеет нулевые значения по всем компонентам времени — `0` часов, `0` минут и `0` секунд. Аналогичное справедливо и при прибавлении временного значения к дате без времени.