## Модуль csv

Несмотря на то что `csv` формат очень прост и мы можем работать с ним, как с обычным текстовым файлом, на практике используется встроенный модуль `csv`.

В данном модуле есть два основных объекта: `reader` и `writer`, созданные, чтобы читать и создавать `csv` файлы соответственно.

### Чтение данных с помощью reader

Для импорта модуля мы используем строку кода:

```
import csv
```

![](https://ucarecdn.com/d1de6ea9-f87a-4c25-a343-f5e443921f97/)    Этот модуль входит в стандартную библиотеку, и его не нужно устанавливать каким‑то особенным способом.

Рассмотрим все тот же файл `products.csv`, содержащий информацию о товарах интернет магазина:

```no-highlight
keywords,price,product_name
Садовый стул,1699,ВЭДДО
Садовый стул,2999,ЭПЛАРО
Садовый табурет,1699,ЭПЛАРО
Садовый стол,1999,ТЭРНО
Складной стол,7499,ЭПЛАРО
Настил,1299,РУННЕН
Стеллаж,1299,ХИЛЛИС
Кружка,39,СТЕЛЬНА
Молочник,299,ВАРДАГЕН
Термос для еды,699,ЭФТЕРФРОГАД
Ситечко,59,ИДЕАЛИСК
Чайник заварочный,499,РИКЛИГ
Кофе-пресс,699,УПХЕТТА
Чашка с блюдцем,249,ИКЕА
Кружка,249,ЭМНТ
Ситечко,199,САККУННИГ
Кружка,199,ФИНСТИЛТ
Тарелка,269,ЭВЕРЕНС
```

Приведенный ниже код:

```python
import csv

with open('products.csv', encoding='utf-8') as file:
    rows = csv.reader(file)                               # создаем reader объект
    for row in rows:
        print(row)
```

читает содержимое файла `products.csv` и выводит:

```no-highlight
['keywords', 'price', 'product_name']
['Садовый стул', '1699', 'ВЭДДО']
['Садовый стул', '2999', 'ЭПЛАРО']
['Садовый табурет', '1699', 'ЭПЛАРО']
['Садовый стол', '1999', 'ТЭРНО']
['Складной стол', '7499', 'ЭПЛАРО']
['Настил', '1299', 'РУННЕН']
['Стеллаж', '1299', 'ХИЛЛИС']
['Кружка', '39', 'СТЕЛЬНА']
['Молочник', '299', 'ВАРДАГЕН']
['Термос для еды', '699', 'ЭФТЕРФРОГАД']
['Ситечко', '59', 'ИДЕАЛИСК']
['Чайник заварочный', '499', 'РИКЛИГ']
['Кофе-пресс', '699', 'УПХЕТТА']
['Чашка с блюдцем', '249', 'ИКЕА']
['Кружка', '249', 'ЭМНТ']
['Ситечко', '199', 'САККУННИГ']
['Кружка', '199', 'ФИНСТИЛТ']
['Тарелка', '269', 'ЭВЕРЕНС']
```

Самая важная строка кода в программе — это строка с созданием `reader` объекта:

```python
rows = csv.reader(file)
```

Объект `reader` дает доступ к построчному итератору, полностью аналогичному работе с файлом или списком.

После выполнения этой строки в переменную `rows` будет записан итератор, с помощью которого можно «пробежаться» циклом по файлу. В каждой итерации цикла при этом будет доступна соответствующая строка файла, уже разбитая по запятым и представляющая собой список. При этом автоматически будут учтены все нюансы с запятыми внутри кавычек и самими кавычками.

Так как каждая строка файла, полученная из итератора, является списком, к ней можно применять все способы работы со списками.

Приведенный ниже код:

```python
import csv

with open('products.csv', encoding='utf-8') as file:
    rows = csv.reader(file)
    for keywords, price, product_name in rows:
        print(f'Ключевые слова: {keywords}, цена: {price}, название: {product_name}')
```

использует распаковку списка и выводит:

```no-highlight
Ключевые слова: keywords, цена: price, название: product_name
Ключевые слова: Садовый стул, цена: 1699, название: ВЭДДО
Ключевые слова: Садовый стул, цена: 2999, название: ЭПЛАРО
Ключевые слова: Садовый табурет, цена: 1699, название: ЭПЛАРО
Ключевые слова: Садовый стол, цена: 1999, название: ТЭРНО
Ключевые слова: Складной стол, цена: 7499, название: ЭПЛАРО
Ключевые слова: Настил, цена: 1299, название: РУННЕН
Ключевые слова: Стеллаж, цена: 1299, название: ХИЛЛИС
Ключевые слова: Кружка, цена: 39, название: СТЕЛЬНА
Ключевые слова: Молочник, цена: 299, название: ВАРДАГЕН
Ключевые слова: Термос для еды, цена: 699, название: ЭФТЕРФРОГАД
Ключевые слова: Ситечко, цена: 59, название: ИДЕАЛИСК
Ключевые слова: Чайник заварочный, цена: 499, название: РИКЛИГ
Ключевые слова: Кофе-пресс, цена: 699, название: УПХЕТТА
Ключевые слова: Чашка с блюдцем, цена: 249, название: ИКЕА
Ключевые слова: Кружка, цена: 249, название: ЭМНТ
Ключевые слова: Ситечко, цена: 199, название: САККУННИГ
Ключевые слова: Кружка, цена: 199, название: ФИНСТИЛТ
Ключевые слова: Тарелка, цена: 269, название: ЭВЕРЕНС
```

При создании `reader` объекта мы можем его настраивать, указывая:

- аргумент `delimiter` — односимвольная строка, используемая для разделения полей, по умолчанию имеет значение `','`
- аргумент `quotechar` — односимвольная строка, используемая для кавычек в полях, содержащих специальные символы, по умолчанию имеет значение `'"'`.

Пусть содержимое файла `products.csv` имеет вид (в качестве разделителя выбран символ `';'`):

```no-highlight
keywords;price;product_name
"Садовый стул, стул для дачи";1699;ВЭДДО
Садовый стул;2999;ЭПЛАРО
Садовый табурет;1699;ЭПЛАРО
Садовый стол;1999;ТЭРНО
"Складной стол, обеденный стол";7499;ЭПЛАРО
Настил;1299;РУННЕН
Стеллаж;1299;ХИЛЛИС
"Кружка, сосуд, стакан с ручкой";39;СТЕЛЬНА
Молочник;299;ВАРДАГЕН
Термос для еды;699;ЭФТЕРФРОГАД
Ситечко;59;ИДЕАЛИСК
Чайник заварочный;499;РИКЛИГ
Кофе-пресс;699;УПХЕТТА
Чашка с блюдцем;249;ИКЕА
"Кружка, стакан с ручкой";249;ЭМНТ
Ситечко;199;САККУННИГ
Кружка;199;ФИНСТИЛТ
"Тарелка, блюдце";269;ЭВЕРЕНС
```

Приведенный ниже код:

```python
import csv

with open('products.csv', encoding='utf-8') as file:
    rows = csv.reader(file, delimiter=';', quotechar='"')
    for index, row in enumerate(rows):
        if index > 5:
            break
        print(row)
```

выводит первые 66 строк файла, включая заголовок с названиями столбцов:

```no-highlight
['keywords', 'price', 'product_name']
['Садовый стул, стул для дачи', '1699', 'ВЭДДО']
['Садовый стул', '2999', 'ЭПЛАРО']
['Садовый табурет', '1699', 'ЭПЛАРО']
['Садовый стол', '1999', 'ТЭРНО']
['Складной стол, обеденный стол', '7499', 'ЭПЛАРО']
```

При создании `reader` объекта мы указываем, что символ-разделитель записей `delimiter` в нашем файле — точка с запятой, а символ кавычек `quotechar` — двойные кавычки. Кроме того, мы используем встроенную функцию `enumerate()` для нумерации строк.

![](https://ucarecdn.com/ad8b3531-946a-407e-9541-3b0547958ac3/)Обратите внимание на то, что для корректной обработки данных мы должны все еще исключить первую строку из обработки, которая содержит названия столбцов.