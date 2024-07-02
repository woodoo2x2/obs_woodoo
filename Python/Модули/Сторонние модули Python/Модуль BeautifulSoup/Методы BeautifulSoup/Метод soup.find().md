`soup.**find()**` - Это метод, который ищет первый элемент, удовлетворяющий заданным критериям, в дереве элементов HTML DOM. Если такой элемент не найден, возвращается `None`.

Синтаксис:

`soup.**find**(**name**, **attrs={}**, **recursive=True**, **string**, **kwargs)`

Параметры: 

Для поиска элементов с помощью метода `.find()` можно использовать различные параметры.

- `**name(**_'div'_**)**` - имя тега элемента, который вы хотите найти. Это может быть строкой или регулярным выражением.
- `**attrs**` - словарь атрибутов элемента, которые вы хотите найти.
- `**recursive**` - если установлено в `False`, метод ищет только в первых дочерних элементах, иначе поиск происходит во всех подэлементах. По умолчанию равен `True`.
- `**string**` - строка, которую вы хотите найти.

Вот примеры, как это можно сделать:

### **Поиск по ID:**

```python
# Здесь 'tag' - это имя тега, а 'your_id' - значение атрибута id.

soup.find('tag', id='your_id')
```

### **Поиск по классу:**

```python
# Заметьте, что используется class_ вместо class, так как class является зарезервированным словом в Python.

soup.find('tag', class_='your_class')
```

### **Поиск по пользовательскому атрибуту:**

```python
# Здесь 'data-attribute' - это имя пользовательского атрибута, а 'value' - его значение.

soup.find('tag', {'data-attribute': 'value'})
```

### **Поиск по двойному пользовательскому атрибуту:**

```python
# Здесь у тега одновременно должны быть два атрибута с заданными значениями.

soup.find('tag', {'data-attribute1': 'value1', 'data-attribute2': 'value2'})
```

### **Поиск по двойному классу:**

```
# Указываются оба класса, разделенные пробелом.

soup.find('tag', class_='class1 class2')
```

### **Поиск по динамическому классу:**

```python
# Если класс формируется динамически и вы знаете только часть названия, можно использовать регулярные выражения:

import re
soup.find('tag', class_=re.compile('your_dynamic_part'))
```

### **Поиск по динамическому пользовательскому атрибуту:**

```python
# 'your_dynamic_part' - часть имени класса.

soup.find('tag', {'data-attribute': re.compile('dynamic_part')})
```

### **Поиск по тегу без атрибутов:**

```python
# Просто укажите имя тега:
# Это найдет первый тег с указанным именем без учета его атрибутов.

soup.find('tag')
```

### **Пример 1**

Код ищет первый тег `<h1>` и первый тег `<p>` с классом `"info"`, и выводит их на экран.

```python
soup.find('h1') 
#и 
soup.find('p', {'class': 'info'})
```

```python
from bs4 import BeautifulSoup

html_doc = """
<html>
    <head>
        <title>Example Page</title>
    </head>
    <body>
        <h1>Hello World</h1>
        <p class="info">This is a paragraph.</p>
        <p class="info">This is another paragraph.</p>
    </body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')

# Найдёт первый тег h1
first_h1 = soup.find('h1')
print(first_h1)

print('----разделитель----')

# Найдёт первый тег p с классом "info"
first_p = soup.find('p', {'class': 'info'})
print(first_p)
```

Вывод:

```
<h1>Hello World</h1>
----разделитель----
<p class="info">This is a paragraph.</p>
```

### **Пример 2**

Используем метод `find()` для нахождения первого `<div>` с атрибутом `id='main'`.

```
main_div = soup.find('div', {'id': 'main'})
```

Далее внутри этого `<div>` используется метод `find()` для поиска первых тегов `<h1>`.

```
main_h1 = main_div.find('h1')
```

Далее ищем `<p>` с классом `'info'`.

```
main_p = main_div.find('p', {'class': 'info'})
```

 Найденные теги выводятся в консоль.

```python
from bs4 import BeautifulSoup

html_doc = """
<html>
    <head>
        <title>Example Page</title>
    </head>
    <body>
        <div id="main">
            <h1>Hello World</h1>
            <p class="info">This is a paragraph.</p>
            <p class="info">This is another paragraph.</p>
            <ul>
                <li>Item 1</li>
                <li>Item 2</li>
                <li>Item 3</li>
            </ul>
        </div>
        <div id="secondary">
            <p>Some additional information.</p>
        </div>
    </body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')

# Находим первый div с id "main"
main_div = soup.find('div', {'id': 'main'})
print(main_div)

print('----разделитель----')

# Найдите первый тег h1 внутри «основного» div
main_h1 = main_div.find('h1')
print(main_h1)

print('----разделитель----')

# Найдите первый тег p с классом «информация» внутри «основного» div
main_p = main_div.find('p', {'class': 'info'})
print(main_p)

print('----разделитель----')

# Найдите первый тег ul внутри «основного» div
main_ul = main_div.find('ul')
print(main_ul)
```

Вывод:

```python
<div id="main">
<h1>Hello World</h1>
<p class="info">This is a paragraph.</p>
<p class="info">This is another paragraph.</p>
<ul>
<li>Item 1</li>
<li>Item 2</li>
<li>Item 3</li>
</ul>
</div>
----разделитель----
<h1>Hello World</h1>
----разделитель----
<p class="info">This is a paragraph.</p>
----разделитель----
<ul>
<li>Item 1</li>
<li>Item 2</li>
<li>Item 3</li>
</ul>


```

### **Пример 3 с применением `attrs`**

Код ищет первый `<div>` с идентификатором `"main"`,

```
main_div = soup.find('div', attrs={'id': 'main'})
```

и первый тег `<p>` с классом `"info"` в объекте `"soup"` и выводит результаты.

```
info_p = soup.find('p', attrs={'class': 'info'})
```

```python
from bs4 import BeautifulSoup

html_doc = """
<html>
    <head>
        <title>Example Page</title>
    </head>
    <body>
        <div id="main">
            <h1>Hello World</h1>
            <p class="info">This is a paragraph.</p>
            <p class="info">This is another paragraph.</p>
            <ul>
                <li>Item 1</li>
                <li>Item 2</li>
                <li>Item 3</li>
            </ul>
        </div>
        <div id="secondary">
            <p>Some additional information.</p>
        </div>
    </body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')

# Найдите первый div с идентификатором «main»
main_div = soup.find('div', attrs={'id': 'main'})
print(main_div)

print('----разделитель----')

# Найдите первый тег p с классом "info"
info_p = soup.find('p', attrs={'class': 'info'})
print(info_p)
```

Вывод:

```
<div id="main">
<h1>Hello World</h1>
<p class="info">This is a paragraph.</p>
<p class="info">This is another paragraph.</p>
<ul>
<li>Item 1</li>
<li>Item 2</li>
<li>Item 3</li>
</ul>
</div>
----разделитель----
<p class="info">This is a paragraph.</p>

Process finished with exit code 0
```

## **`soup.find()`** 

Давайте разбираться как работает этот метод на конкретном примере. 

У нас есть [сайт](http://parsinger.ru/html/index1_page_1.html), и мы хотим получить элемент, который содержит атрибут `class='**item**'`

![](https://ucarecdn.com/a24d3657-9524-4aa0-bc1b-04b0be8ab900/)

```python
from bs4 import BeautifulSoup  
import requests  

# Задаем URL-адрес веб-страницы, с которой будем извлекать данные
url = 'https://parsinger.ru/html/index1_page_1.html'

# Выполняем HTTP-запрос к указанному URL-адресу
response = requests.get(url=url)

# Устанавливаем кодировку ответа в 'utf-8', чтобы корректно отображать кириллицу
response.encoding = 'utf-8'

# Создаем объект BeautifulSoup для анализа HTML-кода страницы
# Второй аргумент 'lxml' указывает на используемый парсер
soup = BeautifulSoup(response.text, 'lxml')

# Ищем на странице первый элемент 'div' с классом 'item'
div = soup.find('div', 'item')

# Выводим найденный элемент на экран
print(div)
```

В этом примере мы ищем элемент `<div>` с классом `item`. Метод `soup.find('div', 'item')` ищет на странице первое вхождение элемента `<div>` с классом 'item' и возвращает его. Если такой элемент не будет найден, то метод вернет `None`.
#ПарсингPython 