## Модуль time

В Python помимо встроенного модуля `datetime` есть еще модуль `time`, который обычно используется при работе с текущим временем.

Работа функций модуля `time` основывается на общепринятой системе описания времени. Согласно ее концепции, текущее время представляется в виде обыкновенного вещественного значения в секундах, прошедших с момента начала эпохи и до сегодняшнего дня. С тех пор это число постоянно растет, позволяя людям работать с различными видами дат в максимально точном представлении.  
  
![](https://ucarecdn.com/4e2e50b8-b96a-4dc7-ac1a-8345d07cb9de/)   Начало эпохи — это полночь 11 января 19701970 года (00:00:00 UTC), когда счетчик секунд имел полностью нулевое значение.

Модуль `time` из стандартной библиотеки языка Python содержит массу полезных функций для работы со временем. С его помощью можно получать информацию о текущих дате и времени, выводить эти сведения в необходимом формате, а также управлять ходом выполнения программы, добавляя задержки по таймеру.

Модуль `time` предоставляет только функции, позволяющие работать со временем.

Использование модуля `time` дает возможность:

- отображать информацию о времени, прошедшем с начала эпохи
- преобразовывать значение системного времени к удобному виду
- прерывать выполнение программы (установка паузы) на заданное количество секунд
- измерять время выполнения программы целиком или ее отдельных модулей

![](https://ucarecdn.com/3dadc6fc-ea66-4794-b8d6-7b66b8ff4ddd/)Обратите внимание, если требуется сравнивать или производить арифметические операции со временем, то нужно использовать модуль `datetime`, а не `time`.

### Функция time()

Для того чтобы получить количество секунд, прошедших с момента начала эпохи, необходимо использовать одноименную функцию `time()` из модуля `time`.

Приведенный ниже код:

```python
import time

seconds = time.time()    # получаем количество прошедших секунд в виде float числа
print('Количество секунд с начала эпохи =', seconds)
```

выводит (на момент запуска 3131 августа 20212021 года):

```no-highlight
Количество секунд с начала эпохи = 1630387918.354396
```

Таким образом, с 11 января 19701970 года прошло уже более 1.61.6 миллиардов секунд.

![](https://ucarecdn.com/4d277568-c065-4eb9-b046-1605532cddb6/)В Python 3.7 добавили функцию `time_ns()`, которая возвращает целочисленное значение, представляющее то же время, прошедшее с эпохи, но в наносекундах, а не в секундах.

### Функция ctime()

Представление времени на основе количества прошедших секунд с момента начала эпохи не очень удобно для человека (хотя и удобно для компьютера). Для того чтобы получить текущую дату в более удобном для человека виде, нужно использовать функцию `ctime()`. Функция `ctime()` принимает в качестве аргумента количество секунд, прошедших с начала эпохи, и возвращает строку, представляющую собой **местное (локальное) время**.

![](https://ucarecdn.com/39af65e9-8b2b-45e8-ac2f-d6f99f1c2ae6/)Представление времени в зависимости от вашего физического местоположения называется местным (локальным) временем и использует концепцию часовых поясов.

Приведенный ниже код:

```python
import time

seconds = 1630387918.354396
local_time = time.ctime(seconds)

print('Местное время:', local_time)
```

выводит:

```no-highlight
Местное время: Tue Aug 31 08:31:58 2021
```

Таким образом, функция `ctime()` возвращает **строковое представление** о местном (локальном) времени, которое включает в себя:

- день недели: `Tue (Tuesday)`
- название месяца: `Aug (August)`
- день месяца: `31`
- часы, минуты, секунды: `08:31:58`
- год: `2021`

Вызывать функцию `ctime()` можно и без аргументов, в этом случае в качестве аргумента подставляется значение вызова функции `time()`. Таким образом, приведенный ниже код:

```python
import time

local_time = time.ctime()              # вызов функции без аргумента
print('Местное время:', local_time)
```

равнозначен коду:

```python
import time

seconds = time.time()
local_time = time.ctime(seconds)
print('Местное время:', local_time)
```

Обратите внимание на то, что результат работы функции `ctime()` зависит от вашего географического положения.

### Функция sleep()

Функция `sleep()` используется для добавления задержки в выполнении программы. Эта функция принимает в качестве аргумента количество секунд (`secs`) и добавляет задержку в выполнении программы на указанное количество секунд.

Рассмотрим программный код:

```python
import time 

print('Before the sleep statement')
time.sleep(3)
print('After the sleep statement')
```

Если вы запустите приведенный выше код, то увидите, что вторая печать выполняется примерно через 33 секунды.

Аргумент `secs` может быть числом с плавающей точкой (`float`), для указания более точного времени приостановки. Например, мы можем сделать задержку на 700700 миллисекунд, что составляет 0.70.7 секунды, как показано ниже:

```python
import time 

print('Before the sleep statement')
time.sleep(0.7)
print('After the sleep statement')
```

![](https://ucarecdn.com/552fd1ff-a11f-406e-97a2-378399639645/)Время приостановки может быть дольше, чем запрошено, на произвольную величину из-за планирования других действий в операционной системе.

Иногда может потребоваться задержка на разное количество секунд. Сделать это можно следующим образом:

```python
import time

for i in [0.7, 0.5, 1.0, 2.5, 3.3]:
    print(f'Waiting for {i} seconds')
    time.sleep(i)
print('The end')
```

Такая программа будет выполняться примерно 0.7+0.5+1.0+2.5+3.3=8.00.7+0.5+1.0+2.5+3.3=8.0 секунд.

![](https://ucarecdn.com/1403f3f6-ed60-4d6c-9000-1b4dadaf606e/)Функция `sleep()` нередко используется для тестирования кода и намеренного внесения задержек на различных этапах выполнения программы.

## Примечания

**Примечание 1.** Документация по модулю `time` на английском языке доступна по [ссылке](https://docs.python.org/3/library/time.html).

**Примечание 2.** Документация по модулю `time` на русском языке доступна по [ссылке](https://docs-python.ru/standart-library/modul-time-python/).

**Примечание 3.** Понятие часового пояса зависит от нашего физического местоположения, однако вы можете изменить его в настройках компьютера без фактического перемещения.

**Примечание 4.** Поскольку местное время связано с нашим языковым стандартом, временные метки часто учитывают специфические для локали детали, такие как порядок элементов в строке и перевод сокращений дня и месяца. Функция `ctime()` игнорирует эти детали.

**Примечание 5.** Обратите внимание на то, что функция `sleep()` фактически останавливает выполнение только текущего потока, а не всей программы. О потоках можно почитать [тут](https://ru.wikipedia.org/wiki/%D0%9F%D0%BE%D1%82%D0%BE%D0%BA_%D0%B2%D1%8B%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F).

**Примечание 6.** Хорошая статья о модуле time доступна по [ссылке](https://proglib.io/p/nazad-v-budushchee-prakticheskoe-rukovodstvo-po-puteshestviyu-vo-vremeni-s-python-2019-12-01).

## Измерение времени выполнения программы

Очень часто нам бывает необходимо измерить время выполнения всей программы, либо отдельных ее частей. Чтобы найти данную величину, достаточно посчитать разницу в секундах между точкой старта и местом, где она завершает свою работу.

В следующем примере демонстрируется применение функции `time()` для получения текущего времени, чтобы в итоге выявить, как долго работал блок кода.

Приведенный ниже код:

```python
import time

start_time = time.time()

for i in range(5): 
    print(i)
    time.sleep(1)

end_time = time.time()

elapsed_time = end_time - start_time
print(f'Время работы программы = {elapsed_time}')
```

выводит (время работы программы может незначительно отличаться):

```no-highlight
0
1
2
3
4
Время работы программы = 5.022884845733643
```

Несмотря на простоту вышеописанного подхода, использовать его в серьезных целях, где требуется точный и независимый от операционной системы (ОС) результат, не рекомендуется. Все дело в том, что числовое значение времени, получаемое таким образом, может иметь погрешности за счет внутренних особенностей работы компьютера, в среде которого выполняется программа. Более того, системные часы могут быть подкорректированы вручную пользователем во время выполнения программы.

![](https://ucarecdn.com/a7786925-aea4-4b8a-9d72-da16e1b66db2/)Может случиться такая ситуация, что очередной вызов функции `time()` вернет значение меньше, чем значение, полученное при предыдущем вызове.

### Функция monotonic()

Для измерения времени выполнения программы идеально подходит функция `monotonic()`, доступная на всех ОС (начиная с Python 3.5), так как ее результат не зависит от корректировки системных часов.

![](https://ucarecdn.com/ee8f36ea-7cf9-42ce-9cce-16178b4bff02/)Функция `monotonic_ns()` похожа на `monotonic()`, но возвращает время в наносекундах. Работает не на всех операционных системах.

Используемый таймер в функции `monotonic()` никогда не вернет при повторном вызове значение, которое будет меньше значения, полученного при предыдущем вызове. Это позволяет избежать многих ошибок, а также неожиданного поведения.

В следующем примере демонстрируется применение функции `monotonic()` для получения текущего времени, чтобы в итоге выявить, как долго работал блок кода.

Приведенный ниже код:

```python
import time

start_time = time.monotonic()

for i in range(5): 
    print(i)
    time.sleep(0.5)

end_time = time.monotonic()

elapsed_time = end_time - start_time
print(f'Время работы программы = {elapsed_time}')
```

выводит (время работы программы может незначительно отличаться):

```no-highlight
0
1
2
3
4
Время работы программы = 2.547000000020489
```

![](https://ucarecdn.com/7999dc52-6eff-474f-9140-8f5a9074324f/)Принцип работы и применения функции `monotonic()` такой же, как и у функции `time()`. Однако функция `monotonic()` дает результат, который обладает гарантированной точностью и не зависит от внешних условий.

### Функция perf_counter()

Для самого точного измерения времени выполнения программы следует использовать функцию `perf_counter()`. Данная функция использует таймер с наибольшим доступным разрешением, что делает эту функцию отличным инструментом для измерения времени выполнения кода на коротких интервалах.

В следующем примере демонстрируется применение функции `perf_counter()` для получения текущего времени, чтобы в итоге выявить, как долго работал блок кода.

Приведенный ниже код:

```python
import time

start_time = time.perf_counter()

for i in range(5): 
    print(i)
    time.sleep(1)

end_time = time.perf_counter()

elapsed_time = end_time - start_time
print(f'Время работы программы = {elapsed_time}')
```

выводит (время работы программы может незначительно отличаться):

```no-highlight
0
1
2
3
4
Время работы программы = 5.042140900040977
```

![](https://ucarecdn.com/02fd4a68-aec5-4538-a02e-1cd603b772b2/)В Python версии 3.7 добавлена функция `perf_counter_ns()` – работает так же, но длительность выводится в наносекундах, что удобнее для совсем малых интервалов времени и быстро исполняемых команд.

## **Примечания** 

**Примечание 1.** О сравнении функций `time()`, `monotonic()`, `perf_counter()` и некоторых других можно почитать почитать [тут](https://superfastpython.com/benchmark-execution-time/).

**Примечание 2.** Для измерения времени работы программы мы также можем использовать встроенный модуль `timeit`. Документация по данному модулю доступна по [ссылке](https://docs-python.ru/standart-library/modul-timeit-python/).

**Примечание 3.** [Небольшая статья](https://www.python.org/doc/essays/list2str/) с общими рекомендациями по оптимизации и сравнениями времени выполнения разных вариаций программ от Гвидо Ван Россума.

## Тип данных struct_time

В модуле `time` имеется единственный тип данных, который называется `struct_time`. Данный тип является именованным кортежем, представляющий информацию о времени. Структура представления времени `struct_time` чем-то похожа на тип `datetime`, который изучался ранее.

![](https://ucarecdn.com/48a65f88-9d57-4eb3-9fd4-7d53186cdf16/)Именованные кортежи будут изучаться позже в рамках этого курса. Именованные кортежи подобны обычным кортежам за тем исключением, что к их полям можно обращаться не только по индексу, но и по названию.

Именованный кортеж `struct_time` состоит из следующих атрибутов:

|Номер индекса|Атрибут|Значение|
|---|---|---|
|0|`tm_year`|диапазон от 0000 до 9999|
|1|`tm_mon`|диапазон от 1 до 12|
|2|`tm_mday`|диапазон от 1 до 31|
|3|`tm_hour`|диапазон от 0 до 23|
|4|`tm_min`|диапазон от 0 до 59|
|5|`tm_sec`|диапазон от 0 до 61|
|6|`tm_wday`|диапазон от 0 до 6, понедельник = 0|
|7|`tm_yday`|диапазон от 1 до 366|
|8|`tm_isdst`|значения -1, 0, 1|
|N/A|`tm_zone`|сокращение названия часового пояса|
|N/A|`tm_gmtoff`|смещение к востоку от UTC в секундах|

Создавать объекты типа `struct_time` можно на основе кортежа:

```python
import time

time_tuple = (2021, 8, 31, 5, 31, 58, 1, 243, 0)
time_obj = time.struct_time(time_tuple)
```

На практике редко приходится собственноручно создавать объекты типа `struct_time`. Обычно используют функции модуля `time`, которые сами создают и оперируют ими. Такие функции как `localtime()`, `gmtime()`, `asctime()` и другие, принимают объект `time.struct_time` в качестве аргумента или возвращают его.

### Функция localtime()

Функция `localtime()` принимает в качестве аргумента количество секунд, прошедших с начала эпохи, и возвращает кортеж `struct_time` в **локальном времени**.

 ![](https://ucarecdn.com/7fe77531-6a31-470b-add7-04f9dcc83579/)   Если функции `localtime()` передан аргумент `None`, то вернется значение `time()`.

Приведенный ниже код:

```python
import time

result = time.localtime(1630387918)
print('Результат:', result)
print('Год:', result.tm_year)
print('Месяц:', result.tm_mon)
print('День:', result.tm_mday)
print('Час:', result.tm_hour)
```

выводит:

```no-highlight
Результат: time.struct_time(tm_year=2021, tm_mon=8, tm_mday=31, tm_hour=8, tm_min=31, tm_sec=58, tm_wday=1, tm_yday=243, tm_isdst=0)
Год: 2021
Месяц: 8
День: 31
Час: 8
```

Обратите внимание на то, что мы можем обращаться к данным именованного кортежа `struct_time` и по индексам.

Приведенный ниже код:

```python
import time

result = time.localtime(1630387918)
print('Результат:', result)
print('Год:', result[0])
print('Месяц:', result[1])
print('День:', result[2])
print('Час:', result[3])
```

аналогичен коду выше, однако менее нагляден.

### Функция gmtime()

Функция `gmtime()` принимает в качестве аргумента количество секунд, прошедших с начала эпохи, и возвращает кортеж `struct_time` **в UTC**.

![](https://ucarecdn.com/eb5f7798-3f01-433a-b811-8389656bc413/)   Если функции `gmtime()` передан аргумент `None`, то вернется значение `time()`.

Приведенный ниже код:

```python
import time

result = time.gmtime(1630387918)
print('Результат:', result)
print('Год:', result.tm_year)
print('Месяц:', result.tm_mon)
print('День:', result.tm_mday)
print('Час:', result.tm_hour)
```

выводит:

```no-highlight
Результат: time.struct_time(tm_year=2021, tm_mon=8, tm_mday=31, tm_hour=5, tm_min=31, tm_sec=58, tm_wday=1, tm_yday=243, tm_isdst=0)
Год: 2021
Месяц: 8
День: 31
Час: 5
```

![](https://ucarecdn.com/9c74fb4f-6ba0-476f-aaa5-2ffcefce6f07/)Обратите внимание на разницу в часах. В Москве используется сдвиг UTC+3:00, поэтому количество часов в локальном времени на 3 больше, чем по UTC.

### Функция mktime()

Функция `mktime()` принимает `struct_time` (или кортеж, содержащий 99 значений, относящихся к `struct_time`) в качестве аргумента и возвращает количество секунд, прошедших с начала эпохи, в местном времени.

Приведенный ниже код:

```python
import time

time_tuple = (2021, 8, 31, 5, 31, 58, 1, 243, 0)
time_obj = time.mktime(time_tuple)
print('Локальное время в секундах:', time_obj)
```

выводит:

```no-highlight
Локальное время в секундах: 1630377118.0
```

Функция `mktime()` является обратной к функции `localtime()`. Следующий пример показывает их связь:

```python
import time

seconds = 1630377118

time_obj = time.localtime(seconds)            # возвращает struct_time
print(time_obj)

time_seconds = time.mktime(time_obj)          # возвращает секунды из struct_time
print(time_seconds)
```

и выводит:

```no-highlight
time.struct_time(tm_year=2021, tm_mon=8, tm_mday=31, tm_hour=5, tm_min=31, tm_sec=58, tm_wday=1, tm_yday=243, tm_isdst=0)
1630377118.0
```

![](https://ucarecdn.com/823e59a4-453a-4eab-9d1b-ee0b840754ab/)Когда кортеж с неправильной длиной или имеющий элементы неправильного типа передается функции, ожидающей `struct_time`, возникает ошибка `TypeError`.

### Функция asctime()

Функция `asctime()` принимает `struct_time` (или кортеж, содержащий 99 значений, относящихся к `struct_time`) в качестве аргумента и возвращает строку, представляющую собой дату и время.

Приведенный ниже код:

```python
import time

time_tuple = (2021, 8, 31, 5, 31, 58, 1, 243, 0)

result = time.asctime(time_tuple)
print('Результат:', result)
```

выводит:

```no-highlight
Результат: Tue Aug 31 05:31:58 2021
```

## Форматированный вывод

Функции `ctime()` и `asctime()` имеют практически одинаковый функционал, за тем исключением, что первая функция принимает количество прошедших от начала эпохи секунд, а вторая принимает `struct_time` (или соответствующий кортеж). Обе функции представляют время в более удобном виде, благодаря автоматическому форматированию.

Приведенный ниже код:

```python
import time

seconds = 1530377118
time_tuple = (2021, 8, 31, 5, 31, 58, 1, 243, 0)

print(time.ctime(seconds))
print(time.asctime(time_tuple))
```

выводит:

```no-highlight
Sat Jun 30 19:45:18 2018
Tue Aug 31 05:31:58 2021
```

С форматированием мы уже сталкивались при работе с типами данных `date, time, datetime`.

Автоматическое форматирование не всегда то, что нужно поскольку может показаться чересчур сложным для восприятия либо же недостаточно информативным. Именно поэтому функции `strftime()` и `strptime()` модуля `time` позволяют создавать свои уникальные типы форматирования.

### Функция strftime()

Функция `strftime` принимает строку с некоторым набором правил для форматирования и объект `struct_time` (или соответствующий кортеж) в качестве аргументов и возвращает строку с датой в зависимости от использованного формата.

Приведенный ниже код:

```python
import time

time_obj = time.localtime()
result = time.strftime('%d.%m.%Y, %H:%M:%S', time_obj)
print(result)
```

выводит (на момент создания урока):

```no-highlight
01.09.2021, 08:50:32
```

### Функция strptime()

Функция `strptime()` делает разбор строки в зависимости от использованного формата и возвращает объект `struct_time`.

Приведенный ниже код:

```python
import time

time_string = '1 September, 2021'
result = time.strptime(time_string, '%d %B, %Y')
print(result)
```

выводит:

```no-highlight
time.struct_time(tm_year=2021, tm_mon=9, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=2, tm_yday=244, tm_isdst=-1)
```

Обратите внимание, что строка `time_string` должна полностью соответствовать формату `%d %B, %Y`, в противном случае возникнет исключение `ValueError`.

## Примечания

**Примечание 1.** Модуль `time` оперирует двумя основными типами объектов: `struct_time` и секундами с начала эпохи.

**Примечание 2.** Таблица для форматированного вывода:

|Формат|Значение|Пример|
|---|---|---|
|`%a`|Сокращенное название дня недели|Sun, Mon, …, Sat (en_US)  <br>Пн, Вт, ..., Вс (ru_RU)|
|`%A`|Полное название дня недели|Sunday, Monday, …, Saturday (en_US)  <br>понедельник, ..., воскресенье (ru_RU)|
|`%w`|Номер дня недели [0, …, 6]|0, 1, …, 6 (0=воскресенье, 6=суббота)|
|`%d`|День месяца [01, …, 31]|01, 02, …, 31|
|`%b`|Сокращенное название месяца|Jan, Feb, …, Dec (en_US);  <br>янв, ..., дек (ru_RU)|
|`%B`|Полное название месяца|January, February, …, December (en_US);  <br>Январь, ..., Декабрь (ru_RU)|
|`%m`|Номер месяца [01, …,12]|01, 02, …, 12|
|`%y`|Год без века [00, …, 99]|00, 01, …, 99|
|`%Y`|Год с веком|0001, 0002, …, 2013, 2014, …, 9999  <br>В Linux год выводится без ведущих нулей:  <br>1, 2, …, 2013, 2014, …, 9999|
|`%H`|Час (24-часовой формат) [00, …, 23]|00, 01, …, 23|
|`%I`|Час (12-часовой формат) [01, …, 12]|01, 02, …, 12|
|`%p`|До полудня или после (при 12-часовом формате)|AM, PM (en_US)|
|`%M`|Число минут [00, …, 59]|00, 01, …, 59|
|`%S`|Число секунд [00, …, 59]|00, 01, …, 59|
|`%f`|Число микросекунд|000000, 000001, …, 999999|
|`%z`|Разница с UTC в формате ±HHMM[SS[.ffffff]]|+0000, -0400, +1030, +063415, ...|
|`%Z`|Временная зона|UTC, EST, CST|
|`%j`|День года [001,366]|001, 002, …, 366|
|`%U`|Номер недели в году  <br>(нулевая неделя начинается с воскр.) [00, …, 53]|00, 01, …, 53|
|`%W`|Номер недели в году  <br>(нулевая неделя начинается с пон.) [00, …, 53]|00, 01, …, 53|
|`%c`|Дата и время в текущей локали|Tue Aug 16 21:30:00 1988 (en_US);  <br>03.01.2019 23:18:32 (ru_RU)|
|`%x`|Дата в текущей локали|08/16/88 (None); 08/16/1988 (en_US);  <br>03.01.2019 (ru_RU)|
|`%X`|Время в текущей локали|21:30:00|

**Примечание 3.** На практике, модуль `time` используется не так часто. В основном для приостановки программы с помощью функции `sleep()` и для измерения времени выполнения программы.