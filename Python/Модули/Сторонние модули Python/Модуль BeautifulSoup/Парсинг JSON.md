[JSON] - **(англ. JavaScript Object Notation)** — текстовый формат обмена данными, основанный на **JavaScript**. При этом формат независим от **JS** и может использоваться в любом языке программирования.

**JSON** входит в стандартный пакет python, устанавливать его не нужно, достаточно просто импортировать  `import json`

Сегодня мы будем рассматривать **JSON** с точки зрения сохранения информации. В этом уроке мы будем говорить о JSON-объектах: как их создавать, сохранять, генерировать и как манипулировать ими.

**JSON**-объект выглядит так, заключается в фигурные скобки. Он очень похож на словарь в Python, если вы с ними уже знакомы, то освоить этот раздел будет очень просто.

```
[{
"name": "Иван",
"age": 27
}]
```

`"name"` - это ключ(**key**), а `"Иван"` - это значение(**value**),  ключ и значение разделяются двоеточием, каждая пара ключ:значение, отделены друг от друга запятой.

Значение(**value**) может быть...

- Строкой;
- Числом;
- True или False;
- Null;
- Другим объектом, в том числе списком или словарем и может иметь какую угодно вложенность. 

Один из простых примеров генерирования **JSON** объекта во время парсинга(#2):

```
import requests
from bs4 import BeautifulSoup
import json

# 1 ------------------------------------------------------
url = 'http://parsinger.ru/html/mouse/3/3_11.html'
response = requests.get(url=url)
response.encoding = 'utf-8'
soup = BeautifulSoup(response.text, 'lxml')
# 1 ------------------------------------------------------

# 2 ------------------------------------------------------
result_json = {
    'name': soup.find('p', id='p_header').text,
    'price': soup.find('span', id='price').text}
# 2 ------------------------------------------------------

# 3 ------------------------------------------------------
with open('res.json', 'w', encoding='utf-8') as file:
    json.dump(result_json, file, indent=4, ensure_ascii=False)
# 3 ------------------------------------------------------


>>> {
    "name": "Мышь Logitech G PRO HERO Black USB проводная",
    "price": "5100 руб"
}
```

В результате выполнения кода мы получили аккуратно собранные данные, которые мы запрашивали во время парсинга:

1. В этом блоке мы совершили `get` запрос на страницу и передали результат запроса в конструктор `BeautifulSoup`;
2. Создали простой словарь `result_json={}` , вручную обозначили ключи, а значения для словаря мы получили из объекта `soup` с помощью тегов;
3. В менеджере контекста `with`  обозначили название файла с расширением `.json`, указали кодировку `"utf-8"`.

### Методы `JSON`:

- `json.dump()` - преобразует объекты **python** (в нашем примере это словарь) в соответствующий объект **JSON**. Метод `.dump()` первым параметром ожидает словарь, который мы будем записывать в файл, а вторым параметром - файл, куда мы будем записывать наш словарь.
- 1. `indent=4` улучшает читаемость файла `json`, и обозначает отступ в пробелах;
    2. `ensure_ascii=False` - если не указать, могут возникнуть проблемы с кодировкой. Если установить значение `True`, то кириллические символы будут отображены в **ascii**, примерно вот так `\u041c\u044b\u0448\u044c`.
- `json.dumps()` - отличается лишь тем, что кодирует наши данные в **Python string**  и служит для преобразования примитивных типов данных. В ваших парсерах вы, скорее всего, будете использовать именно первый вариант;
- `json.load()` - метод считывает файл в формате **JSON** и возвращает объекты Python, про метод **load()** подробнее мы поговорим позже когда, будем считывать **json**-файлы;
- `json.load**s**()` - метод считывает **строку** в формате JSON и возвращает объекты Python.

Давайте разберем еще один пример кода по извлечению данных с сайта и формированию словаря для дальнейшего дампа в `.json.`

```
import requests
from bs4 import BeautifulSoup
import json

# 1 ------------------------------------------------------
url = 'http://parsinger.ru/html/index3_page_1.html'
response = requests.get(url=url)
response.encoding = 'utf-8'
soup = BeautifulSoup(response.text, 'lxml')
# 1 ------------------------------------------------------

# 2 ------------------------------------------------------
name = [x.text.strip() for x in soup.find_all('a', class_='name_item')]
description = [x.text.strip().split('\n') for x in soup.find_all('div', class_='description')]
price = [x.text for x in soup.find_all('p', class_='price')]
# 2 ------------------------------------------------------

result_json = []
# 3 ------------------------------------------------------
for list_item, price_item, name in zip(description, price, name):
    result_json.append({
        'name': name,
        'brand': [x.split(':')[1] for x in list_item][0],
        'type': [x.split(':')[1] for x in list_item][1],
        'connect': [x.split(':')[1] for x in list_item][2],
        'game': [x.split(':')[1] for x in list_item][3],
        'price': price_item

    })

# 3 ------------------------------------------------------

# 4 ------------------------------------------------------
with open('res.json', 'w', encoding='utf-8') as file:
    json.dump(result_json, file, indent=4, ensure_ascii=False)
# 4 ------------------------------------------------------
```

В этом коде активно применялись list comprehension для извлечения необходимых данных и формирования списка словарей.

1. В блоке №1 нет ничего нового для вас;
2. В блоке №2 мы извлекаем информацию с каждой карточки на сайте [тренажёре](http://parsinger.ru/html/index3_page_1.html), извлекаем наименование товара, его описание и стоимость. Если мы посмотрим на элементы страницы HTML, мы увидим,  что `description`  извлекается методом `find_all()` и получается список списков, который необходимо записать в наш список словарей;
3. В этом блоке мы инициируем цикл, в котором проходимся по трём основным спискам, мы создали их в блоке 2, и на каждой итерации записываем значение в соответствующий ключ нашего словаря. 

В результате выполнения этого кода мы получаем файл `res.json` в каталоге с нашим проектом, который выглядит вот так:

![](https://ucarecdn.com/19bb0a82-cd51-4b84-8d2f-21681e010228/)

Скопируйте код выше и запустите у себя на компе, понаблюдайте за рез
ультатом.


В этой части мы поговорим о том, как извлекать значения атрибута. Именно его значение, а не текст, который заключен в теге. Этим методом можно собирать любые значения атрибутов, `class=""`, `name=""`, `src=""`, `href=""`, `id=""`, не имеет значения какой тег. Чтоб далеко не ходить, мы будем тренироваться на нашем [тренажёре](http://parsinger.ru/html/watch/1/1_1.html).

![](https://ucarecdn.com/cbef70c3-7476-4d7a-988e-d0a9c2f5f815/)

В результате выполнения кода у нас получится вот такой список:  `['brand', 'model', 'type', 'display', 'material_frame', 'material_bracer', 'size', 'site']`. Это понадобится вам для решения задачи, которая ждет вас далее.

```python
import requests
from bs4 import BeautifulSoup

response = requests.get('http://parsinger.ru/html/watch/1/1_1.html')
response.encoding = 'utf-8'
soup = BeautifulSoup(response.text, 'lxml')
description = soup.find('ul', id='description').find_all('li')

for li in description:
    print(li['id'])


>>> brand
    model
    type
    display
    material_frame
    material_bracer
    size
    site
```

В первую очередь нам необходимо найти и получить родительский элемент, в этом примере это `<ul id="description">`, `description = soup.find('ul', id='description').find_all('li')` - будет хранить список всех дочерних элементов `<li>`, давайте на них посмотрим.

![](https://ucarecdn.com/64e9ae38-6b8b-45b9-816a-6fb3502ace68/)

Далее мы проходимся по каждому элементу в этом списке и обращаемся с ним как с элементами словаря - `li['id']`. Мы можем добавлять элементы в список на каждой итерации, а можем использовать **list comprehension** `li_id = [x['id'] for x in description]` и получить готовый список без лишнего кода.

```python
import requests
from bs4 import BeautifulSoup

response = requests.get('http://parsinger.ru/html/watch/1/1_1.html')
response.encoding = 'utf-8'
soup = BeautifulSoup(response.text, 'lxml')
description = soup.find('ul', id='description').find_all('li')
li_id = [x['id'] for x in description]
print(li_id)


>>> ['brand', 'model', 'type', 'display', 'material_frame', 'material_bracer', 'size', 'site']


```

Очень часто сервера отдают нужную нам информацию в **JSON**-формате, и наша задача - найти именно тот сетевой запрос, в  котором есть **JSON**, и распарсить его. 

Как найти "**тот самый запрос с JSON**".

- Использование инструментов разработчика в браузере для мониторинга сетевых запросов **F12** => Вкладка **network** => Фильтр **Fetch\XHR**.
- Изучение документации к API для определения, какие запросы возвращают данные в формате JSON (Если такая имеется, например тут [API](https://stepik.org/api/docs/) stepik).

Для начала нужно понять, как находить те самые запросы, в которых есть ответ в **JSON**. Посмотрим на Российский маркетплейс, **wildberries**.

![](https://ucarecdn.com/84ac713b-5927-453e-9c1b-54f7d784c769/)

Откройте [wildberries](https://www.wildberries.ru/catalog/elektronika/igry-i-razvlecheniya/aksessuary/garnitury), найдите любой JSON [запрос](https://static-basket-01.wb.ru/vol0/data/main-menu-ru-ru-v2.json) и скопируйте содержимое поля **JSON** на сайт [jsonviewer](http://jsonviewer.stack.hu/) для удобного отображения, или откройте комментарии к уроку, там уже есть и другие рекомендации по плагинам к хрому. Мы увидим хорошо структурированную информацию. С такой информацией работать намного проще. 

![](https://ucarecdn.com/d7f206e9-fd14-4e5f-a9c5-3c1569041367/)

Давайте внимательно рассмотрим ссылку, обнаруженную на первом скриншоте. В этой ссылке мы видим параметр `"page=2"`. Именно этот параметр будет нам необходим для навигации по всем доступным страницам. В процессе каждой итерации цикла мы будем получать JSON-файл, содержащий карточки товаров. Вы можете лично убедиться в этом, изменяя вручную значение параметра на `"page=2"` или другое число. На момент создания данного материала, на сайте доступно девять страниц.

Вот [ссылка](https://www.wildberries.ru/catalog/elektronika/planshety?sort=popular&page=2), на момент создания этого степа там девять страниц.


![](https://ucarecdn.com/7f0b114d-540f-43e8-8157-f70de0d3f9a3/)




## Парсим JSON часть 2

Для следующего примера нам понадобится [jsonplaceholder](https://jsonplaceholder.typicode.com/posts), сервис, который предоставляет JSON для разработчиков. У него есть 6 ресурсов -. [/posts](https://jsonplaceholder.typicode.com/posts), [/comments](https://jsonplaceholder.typicode.com/comments), [/albums](https://jsonplaceholder.typicode.com/albums), [/photos](https://jsonplaceholder.typicode.com/photos), [/todos](https://jsonplaceholder.typicode.com/todos),  [/users](https://jsonplaceholder.typicode.com/users)

Давайте сделаем запрос на [/posts](https://jsonplaceholder.typicode.com/posts) и посмотрим на данные через [jsonviewer](http://jsonviewer.stack.hu/). Мы увидим 100 постов, где мы можем извлечь четыре поля: `"userId"`, `"id"`, `"title"`, `"body"`. Если мы хотим получить какой-либо определенный элемент, мы должны указать его в квадратных скобках- `["title"]`.

![](https://ucarecdn.com/b4c63b18-6e02-415f-8a07-e6a618116d5b/)

Для того, чтобы получить все `'userId'` и `'title'`, мы пройдемся по всем элементам в цикле `for`:

```
import requests

url = 'https://jsonplaceholder.typicode.com/posts'

response = requests.get(url=url).json()
for item in response:
    print(item["userId"], item["title"])


>>> 1 sunt aut facere repellat provident occaecati excepturi optio reprehenderit
    1 qui est esse
    1 ea molestias quasi exercitationem repellat qui ipsa sit au
    ...
    10 at nam consequatur ea labore ea harum
```

В результате мы получили все `"userId"` и `"title"`. Обратите внимание на метод `.json()`, применяемый к переменной `response`. Мы рассмотрели метод `.json()` в разделе, посвященном библиотеке `requests`. Данный метод позволяет сериализовать информацию, полученную с сервера, в формате JSON-объекта, при условии, что сервер готов предоставить нам эти данные.

### Вложенность JSON

 

Наша цель — извлечь из значения ключа `'description'` вложенные ключи `'brand'` и `'model'`. Для достижения этой цели, первым в списке указывается родительский ключ, а затем — дочерний. В нашем случае, это будет выглядеть как `['description']['brand']`.

![](https://ucarecdn.com/ac6916d5-686f-42b4-a8e6-104685bf4de3/)

```
import requests

url = 'http://parsinger.ru/downloads/get_json/res.json'

response = requests.get(url=url).json()
for item in response:
    print(item["description"]["brand"], item["description"]["model"])

>>> Jet Excidium
    Huawei Band 6 FRA-B19
    Huawei Band 6 FRA-B19
    Huawei GT 3 MIL-B19V
    ...
    HP Pavilion Gaming 600
```
#ПарсингPython 