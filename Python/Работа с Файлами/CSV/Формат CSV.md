Формат CSV

CSV (от англ. Comma-Separated Values — значения, разделённые запятыми) — текстовый формат, предназначенный для представления табличных данных. Строка таблицы соответствует строке текста, которая содержит одно или несколько полей, разделенных запятыми.

Например, таблица:

|Rank|Language|Share|
|---|---|---|
|1|Python|31.17%|
|2|Java|17.75%|
|3|JavaScript|8%|
|4|C#|7.05%|
|5|PHP|6.09%|

в формате `csv` будет выглядеть так:

```no-highlight
Rank,Language,Share
1,Python,31.17%
2,Java,17.75%
3,JavaScript,8%
4,C#,7.05%
5,PHP,6.09%
```

  Обратите внимание, пробелов после запятой быть не должно.

## Ручная работа с файлами

Рассмотрим текстовый файл `products.csv`, содержащий информацию о товарах некоторого интернет магазина. Файл содержит информацию о трех столбцах:

- `keywords` (ключевые слова)
- `price` (цена)
- `product_name` (имя продукта)

и имеет следующее содержимое:

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

Разделителем записей был выбран символ **новой строки**, а разделителем полей — **символ запятой.**

В результате мы получили текстовый файл, который легко читается и человеком и компьютерной программой.

Поскольку любой `csv` файл является текстовым, то нам не составит труда обработать его "руками", подобно тому, как мы обрабатываем обычный текстовый файл.

Приведенный ниже код:

```python
with open('products.csv', encoding='utf-8') as file:
    data = file.read()
    for line in data.splitlines():
        print(line.split(','))
```

выводит:

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
   Для построчного разделения текста удобно использовать строковый метод `splitlines()`, вместо метода `split('\n')`.

Мы также можем создать вложенный список (таблицу) для более удобного взаимодействия с данными:

```python
with open('products.csv', encoding='utf-8') as file:
    data = file.read()
    table = [r.split(',') for r in data.splitlines()]
```

С помощью такой таблицы мы можем обратиться к цене седьмого по счету товара:

```
print(table[7][1])
```

Или мы можем также отсортировать товары по цене и напечатать 55 самых дешевых товаров.

Приведенный ниже код:

```python
with open('products.csv', encoding='utf-8') as file:
    data = file.read()
    table = [r.split(',') for r in data.splitlines()]
    del table[0]                                        # удаляем заголовок
    table.sort(key=lambda item: int(item[1]))
    for line in table[:5]:
        print(line)
```

выводит:

```no-highlight
['Кружка', '39', 'СТЕЛЬНА']
['Ситечко', '59', 'ИДЕАЛИСК']
['Ситечко', '199', 'САККУННИГ']
['Кружка', '199', 'ФИНСТИЛТ']
['Чашка с блюдцем', '249', 'ИКЕА']
```

Обратите внимание на то, что мы удалили первый элемент из списка, так как он содержит не данные, а заголовки. Также стоит обратить внимание на то, что в лямбда-функции мы преобразуем элемент `item[1]` к числовому типу, иначе сортировка будет работать не так, как полагается.

В приведенном выше файле `products.csv` символ запятой использовался в качестве разделителя полей. Однако бывают ситуации, в которых сам символ запятой является легитимным символом. Например, столбец `keywords` может содержать (и как правило на практике содержит) несколько ключевых слов. В таком случае структура файла нарушается. Для решения такой проблемы можно использовать два подхода.

**1 подход.** Если поле содержит запятые, то это поле должно быть заключено в двойные кавычки. Если этого не сделать, то данные невозможно будет корректно обработать.

Содержимое файла `products.csv` может иметь вид:

```no-highlight
keywords,price,product_name
"Садовый стул, стул для дачи",1699,ВЭДДО
Садовый стул,2999,ЭПЛАРО
Садовый табурет,1699,ЭПЛАРО
Садовый стол,1999,ТЭРНО
"Складной стол, обеденный стол",7499,ЭПЛАРО
Настил,1299,РУННЕН
Стеллаж,1299,ХИЛЛИС
"Кружка, сосуд, стакан с ручкой",39,СТЕЛЬНА
Молочник,299,ВАРДАГЕН
Термос для еды,699,ЭФТЕРФРОГАД
Ситечко,59,ИДЕАЛИСК
Чайник заварочный,499,РИКЛИГ
Кофе-пресс,699,УПХЕТТА
Чашка с блюдцем,249,ИКЕА
"Кружка, стакан с ручкой",249,ЭМНТ
Ситечко,199,САККУННИГ
Кружка,199,ФИНСТИЛТ
"Тарелка, блюдце",269,ЭВЕРЕНС
```

Обратите внимание на записи с номерами 11, 55, 88, 1515 и 1818. В этих записях в столбце `keywords` используется символ запятой, и для правильной обработки данных нам необходимо соответствующее поле обрамить символом двойных кавычек.

Обратите внимание на то, что при таком содержимом файла `products.csv` наш код, написанный выше, является нерабочим, так как он разделит каждую строку через символ запятой.

**2 подход.** Использовать в качестве разделителя другой символ, например, символ табуляции`(\t)`, который весьма редко встречается в качестве валидного содержимого файла.

```no-highlight
keywords	price	product_name
Садовый стул, стул для дачи	1699	ВЭДДО
Садовый стул	2999	ЭПЛАРО
Садовый табурет	1699	ЭПЛАРО
Садовый стол	1999	ТЭРНО
Складной стол, обеденный стол	7499	ЭПЛАРО
Настил	1299	РУННЕН
Стеллаж	1299	ХИЛЛИС
Кружка, сосуд, стакан с ручкой	39	СТЕЛЬНА
Молочник	299	ВАРДАГЕН
Термос для еды	699	ЭФТЕРФРОГАД
Ситечко	59	ИДЕАЛИСК
Чайник заварочный	499	РИКЛИГ
Кофе-пресс	699	УПХЕТТА
Чашка с блюдцем	249	ИКЕА
Кружка, стакан с ручкой	249	ЭМНТ
Ситечко	199	САККУННИГ
Кружка	199	ФИНСТИЛТ
Тарелка, блюдце	269	ЭВЕРЕНС
```

Формат данных `csv`, в котором разделителем является символ табуляции, называют `tsv` (англ. tab separated values — «значения, разделенные табуляцией»).

`tsv` — текстовый формат для представления табличных данных. Каждая запись в таблице — строка текстового файла. Каждое поле записи отделяется от других символом табуляции, а точнее — горизонтальной табуляции.



`tsv` и `csv` — формы более общего формата `dsv` (англ. delimiter separated values — «значения, разграниченные разделителем»).

Приведенный ниже код правильно обрабатывает файл `products.tsv`, в котором символом разделителя выбран символ табуляции (`\t`):

```python
with open('products.tsv', encoding='utf-8') as file:
    data = file.read()
    table = [r.split('\t') for r in data.splitlines()]
    del table[0]
    table.sort(key=lambda item: int(item[1]))
    for line in table[:5]:
        print(line)
```

и выводит 5 самых дешевых товаров:

```no-highlight
['Кружка, сосуд, стакан с ручкой', '39', 'СТЕЛЬНА']
['Ситечко', '59', 'ИДЕАЛИСК']
['Ситечко', '199', 'САККУННИГ']
['Кружка', '199', 'ФИНСТИЛТ']
['Чашка с блюдцем', '249', 'ИКЕА']
```
#Python 