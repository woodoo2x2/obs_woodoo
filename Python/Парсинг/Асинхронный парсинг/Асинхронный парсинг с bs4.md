Самая долгожданная часть асинхронного модуля. В этом разделе мы наконец-то напишем свой первый асинхронный парсер, который ускорит сбор информации в десятки раз с помощью [[Модуль BeautifulSoup4]].

Для начала напишем самый простой парсер, который соберёт с одной страницы нашего сайта-тренажера всего лишь названия и цены. Большую часть кода вы уже видели, и в этом примере всё будет вам очень знакомо.

Этот пример важен для понимания того, как мы будем строить дальнейшее обучение.

```python
import aiohttp
import asyncio
from bs4 import BeautifulSoup


async def main():
    url = 'https://parsinger.ru/html/index1_page_1.html'
    async with aiohttp.ClientSession() as session:
        async with session.get(url=url, timeout=1) as response:
            soup = BeautifulSoup(await response.text(), 'lxml')
            name = soup.find_all('a', class_='name_item')
            price = soup.find_all('p', class_='price')
            for n, p in zip(name, price):
                print(n.text, p.text)


asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())

Результат:
    Jet Kid Start blue Умные детские часы 2310 руб
    BAND 6 FOREST GREEN FARA-B19 HUAWEI 5480 руб
    Умные часы GT 3 MIL-B19S BLACK HUAWEI 21810 руб
    Умные часы GT 3 MIL-B19V BROWN HUAWEI 21810 руб
    GT RUNNER-B19S BLACK HUAWEI 27770 руб
    GT RUNNER-B19A GREY HUAWEI 27770 руб
    Умные часы GT 3 MIL-B19 GOLD HUAWEI 24230 руб
    Умные часы WATCH 3 GALILEO-L11 STEEL 32600 руб
```

Всё очень просто и понятно. К этому этапу курса вы уже умеете работать с "супом", и в асинхронном коде это почти не отличается от синхронного стиля, за некоторыми исключениями. К примеру, появилось ключевое слово `await`, которое располагается перед ответом переменной `await response.text()`. В этом месте происходит отправка запроса и получение ответа от сервера, поэтому нам нужно написать ключевое слово `await`, чтобы дать понять нашему циклу событий, где нам нужно переключаться, пока мы ожидаем ответ от сервера.

```
soup = BeautifulSoup(await response.text(), 'lxml')
```

Следующий пример будет немного сложнее, так как мы будем получать с нашего [сайта](https://parsinger.ru/html/index1_page_1.html)-тренажера информацию с карточек, которых там 160 штук: цену и наименование товара.

Запустите код у себя в терминале и попробуйте понять, что мы тут написали. Если всё равно не понятно, то ниже будет полное описание.

```python
import asyncio
import time
import aiohttp
from bs4 import BeautifulSoup

# ---------------------start block 1------------------------
category = ['watch', 'mobile', 'mouse', 'hdd', 'headphones']
urls = [f'https://parsinger.ru/html/{cat}/{i}/{i}_{x}.html' for cat, i in zip(category, range(1, len(category) + 1)) for
        x in range(1, 33)]
# ---------------------end block 1------------------------

# ---------------------start block 2------------------------
async def run_tasks(url, session):
    async with session.get(url) as resp:
        soup = BeautifulSoup(await resp.text(), 'lxml')
        price = soup.find('span', id='price').text
        name = soup.find('p', id='p_header').text
        print(resp.url, price, name)
# ---------------------end block 2------------------------

# ---------------------start block 3------------------------
async def main():
    async with aiohttp.ClientSession() as session:
        tasks = [run_tasks(link, session) for link in urls]
        await asyncio.gather(*tasks)
# ---------------------end block 3------------------------

# ---------------------start block 4------------------------

if __name__ == '__main__':
    asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
    start = time.time()
    asyncio.run(main())
    print(time.time()-start)
```

1.  Блок кода №1. 
    - В этом блоке мы проанализировали наш [сайт](https://parsinger.ru/html/index1_page_1.html) и поняли, что проще всего будет подготовить все ссылки заранее и передать их все сразу в цикл событий. Это самый простой, но не совсем надежный способ, потому что этот код может собрать только то, что мы сгенерировали. Если на сайте появится дополнительная категория или страница пагинации, код до них не доберётся. Тем не менее, этот подход имеет право на существование из-за своей простоты. Вот так выглядит сгенерированный список ссылок.
        
        ```
        https://parsinger.ru/html/watch/1/1_1.html
        https://parsinger.ru/html/watch/1/1_2.html
        https://parsinger.ru/html/watch/1/1_3.html
        ...
        https://parsinger.ru/html/headphones/5/5_30.html
        https://parsinger.ru/html/headphones/5/5_31.html
        https://parsinger.ru/html/headphones/5/5_32.html
        ```
        
2. Блок кода № 2.
    - Здесь всё очень знакомо и просто: вы видели это в предыдущем разделе, посвящённом aiohttp, и по предыдущему примеру кода всё должно быть понятно. Вы уже неоднократно работали с BeautifulSoup, так что вопросов быть не должно.
3. Блок кода № 3.

- А вот тут начинается самое интересное.

1. **Создание клиентской сессии aiohttp**:
    
    - `async with aiohttp.ClientSession() as session`: Здесь создается асинхронный контекстный менеджер, который открывает сессию клиента `aiohttp`. Объект `session` представляет собой сессию клиента, которая может быть использована для выполнения HTTP-запросов. Использование сессии в контекстном менеджере гарантирует, что сессия будет корректно закрыта после выхода из блока `async with`.
2. **Создание списка корутин**:
    
    - `tasks = [run_tasks(link, session) for link in urls]`: В этой строке создается список корутин. Каждый элемент списка — это объект корутины, который получается в результате вызова асинхронной функции `run_tasks` с соответствующими параметрами(ссылка на страницу и объект сессии). Эти объекты корутин еще не выполняются, они представляют собой запланированные для выполнения операции.  
          
        *  **Корутина в Python** — это функция, определенная с использованием `async def`, и она при вызове возвращает объект корутины. Однако, корутина сама по себе не выполняется автоматически, она должна быть запущена в цикле событий.  
        Задача (`asyncio.Task`) — это обертка вокруг корутины, которая позволяет ей выполняться асинхронно.
    
    ```python
    async def run_tasks():
        tasks = [main(link) for link in urls]
        for x in task:
            print(type(x))
        await asyncio.gather(*tasks)
    
    Результат:
        <class 'coroutine'>
        <class 'coroutine'>
        ...
        <class 'coroutine'>
        <class 'coroutine'>
    ```
    
3. **Планирование выполнения корутин**:
    
    - `await asyncio.gather(*tasks)`: Здесь `asyncio.gather` принимает список корутин и планирует их параллельное выполнение. Корутины из списка `tasks`  автоматически преобразуются в задачи.  
        Функция `await asyncio.gather(*tasks)` запускает все задачи одновременно, а ключевое слово `await` говорит программе, что необходимо дождаться результата выполнения каждой задачи.
4. Блок кода № 4.
    
    ```
    ​​​​​​​if __name__ == '__main__':
        asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
        start = time.time()
        asyncio.run(main())
        print(time.time()-start)
    ```
    
    Функция `asyncio.run(main())`, запускает цикл событий с корутиной `main()`, в качестве основной задачи.  
    Так же добавлено измерение времени работы скрипта.  
     

Вывод:

```python
https://parsinger.ru/html/watch/1/1_3.html 21810 руб Умные часы GT 3 MIL-B19S BLACK HUAWEI
https://parsinger.ru/html/watch/1/1_5.html 27770 руб Умные часы GT RUNNER-B19S BLACK HUAWEI
https://parsinger.ru/html/mobile/2/2_2.html 1070 руб Мобильный телефон Fly Ezzy 3 белый
...............

https://parsinger.ru/html/headphones/5/5_26.html 4220 руб ИГРОВАЯ USB-ГАРНИТУРА С ТЕХНОЛОГИЕЙ ОБЪЕМНОГО ЗВУЧАНИЯ HARRIER 360
https://parsinger.ru/html/headphones/5/5_24.html 830 руб Наушники Ritmix RH-540M
https://parsinger.ru/html/headphones/5/5_28.html 1620 руб Гарнитура Defender Lester
0.5940024852752686
```

Если вы запускали код у себя в терминале, то, возможно, обратили внимание на то, что ссылки в консоли печатаются в случайном порядке. Случайный порядок возвращаемых результатов обусловлен спецификой асинхронной обработки задач.  
Результат печатается сразу, как только задача окажется выполненной.  
Если вам нужен список результатов расположенных в строгом порядке, то нужно изменить `run_tasks()` так, чтобы вместо `print()` был использован `return`:

```python
async def run_tasks(url, session):
    async with session.get(url) as resp:
        soup = BeautifulSoup(await resp.text(), 'lxml')
        price = soup.find('span', id='price').text
        name = soup.find('p', id='p_header').text
        return resp.url, price, name
```

А в main() сохраним результаты работы всех задач в список result и распечатаем содержимое этого списка:

```python
async def main():
    async with aiohttp.ClientSession() as session:
        tasks = [run_tasks(link, session) for link in urls]
        # Сохраняем результаты работы всех задач в result
        result = await asyncio.gather(*tasks)
        # Распечатываем содержимое списка result
        for x in result:
            print(*x, sep=', ')
```

Вывод:

```
https://parsinger.ru/html/watch/1/1_1.html, 2310 руб, Jet Kid Start blue Умные детские часы
https://parsinger.ru/html/watch/1/1_2.html, 5480 руб, Фитнес- браслет BAND 6 FOREST GREEN FARA-B19 HUAWEI
https://parsinger.ru/html/watch/1/1_3.html, 21810 руб, Умные часы GT 3 MIL-B19S BLACK HUAWEI
https://parsinger.ru/html/watch/1/1_4.html, 21810 руб, Умные часы GT 3 MIL-B19V BROWN HUAWEI
https://parsinger.ru/html/watch/1/1_5.html, 27770 руб, Умные часы GT RUNNER-B19S BLACK HUAWEI
.........................
```

Обратите теперь внимание на нумерацию страниц.

В этой части мы разберем на практике как динамически получать разделы и пагинацию и работать со всем этим асинхронно. Сразу хочу предупредить, что все примеры в этом степе будут написаны в простом функциональном стиле, хотя для этого идеально подходит ОПП. Это сделано для того, чтобы вам не пришлось дополнительно разбираться в ООП, а если вы уже знаете его, то для вас не будет сложности переписать этот код так, как вы привыкли. Все-таки, подавляющее большинство учеников курса - начинающие, и не очень хочется еще сильнее усложнять и без того сложную тему.

Этот код с **docstring** можно посмотреть на [github.com](https://github.com/nefelsay/stepik_parsing/blob/main/create_async_soup.py).

Этот код уже содержит в себе динамический сбор страниц пагинации в каждой категории. Тут у нас есть три синхронные функции, которые нам помогают это сделать, но их мы не будем запускать в цикле событий. 

```
import aiohttp
import asyncio
import requests
from bs4 import BeautifulSoup

category_lst = []
pagen_lst = []
domain = 'https://parsinger.ru/html/'


def get_soup(url):
    resp = requests.get(url=url)
    return BeautifulSoup(resp.text, 'lxml')


def get_urls_categories(soup):
    all_link = soup.find('div', class_='nav_menu').find_all('a')

    for cat in all_link:
        category_lst.append(domain + cat['href'])


def get_urls_pages(category_lst):
    for cat in category_lst:
        resp = requests.get(url=cat)
        soup = BeautifulSoup(resp.text, 'lxml')
        for pagen in soup.find('div', class_='pagen').find_all('a'):
            pagen_lst.append(domain + pagen['href'])


async def get_data(session, link):
    async with session.get(url=link) as response:
        resp = await response.text()
        soup = BeautifulSoup(resp, 'lxml')
        item_card = [x['href'] for x in soup.find_all('a', class_='name_item')]
        for x in item_card:
            url2 = domain + x
            async with session.get(url=url2) as response2:
                resp2 = await response2.text()
                soup2 = BeautifulSoup(resp2, 'lxml')
                article = soup2.find('p', class_='article').text
                name = soup2.find('p', id='p_header').text
                price = soup2.find('span', id='price').text
                print(url2, price, article, name)


async def main():
    async with aiohttp.ClientSession() as session:
        tasks = []
        for link in pagen_lst:
            task = asyncio.create_task(get_data(session, link))
            tasks.append(task)
        await asyncio.gather(*tasks)


url = 'https://parsinger.ru/html/index1_page_1.html'
soup = get_soup(url)
get_urls_categories(soup)
get_urls_pages(category_lst)

asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
```

 Итак, давайте подробнее поговорим про каждую из функций, сверху-вниз.

1. Функция `def get_soup(url)`:
    - ```
        def get_soup(url):
            resp = requests.get(url=url)
            return BeautifulSoup(resp.text, 'lxml')
        ```
        
        Эта функция максимально проста, она извлекает суп из переданной в нее  первый ссылки на сайте `https://parsinger.ru/html/index1_page_1.html`.
2. Функция `get_urls_categories(soup)`:
    - ```
        def get_urls_categories(soup):
            all_link = soup.find('div', class_='nav_menu').find_all('a')
            for cat in all_link:
                category_lst.append(domain + cat['href'])
        ```
        
        Эта функция извлекает все ссылки из правого списка с навигацией, в которой находятся все категории товаров, также она наполняет глобальный список `category_lst = []` ссылками всех категорий. На нашем сайте-тренажере всего пять категорий, и все пять ссылок попадают в этот глобальный список. Он нам потребуется для извлечения длины пагинации в каждой из этих категорий.
        
        ```
        category_lst = ['https://parsinger.ru/html/index1_page_1.html',
                        'https://parsinger.ru/html/index2_page_1.html',
                        'https://parsinger.ru/html/index3_page_1.html',
                        'https://parsinger.ru/html/index4_page_1.html',
                        'https://parsinger.ru/html/index5_page_1.html',
                        ]
        ```
        
3. Функция `def get_urls_pages(category_lst):`
    - Эта функция собирает все ссылки пагинации, которые находятся в каждой из категорий. Также эта функция формирует глобальный список `pagen_lst = [],` на каждой из этих ссылок находится по восемь карточек с товаром, из которых мы будем извлекать данные в асинхронных функциях.
        
        ```
        pagen_lst = ['https://parsinger.ru/html/index1_page_1.html', 
                       'https://parsinger.ru/html/index1_page_2.html',
                       'https://parsinger.ru/html/index1_page_3.html',                               
                       'https://parsinger.ru/html/index1_page_4.html',
                       'https://parsinger.ru/html/index2_page_1.html', 
                       'https://parsinger.ru/html/index2_page_2.html',
                       'https://parsinger.ru/html/index2_page_3.html', 
                       'https://parsinger.ru/html/index2_page_4.html',
                       'https://parsinger.ru/html/index3_page_1.html',                
                       'https://parsinger.ru/html/index3_page_2.html',
                       'https://parsinger.ru/html/index3_page_3.html', 
                       'https://parsinger.ru/html/index3_page_4.html',
                       'https://parsinger.ru/html/index4_page_1.html', 
                       'https://parsinger.ru/html/index4_page_2.html',
                       'https://parsinger.ru/html/index4_page_3.html', 
                       'https://parsinger.ru/html/index4_page_4.html',
                       'https://parsinger.ru/html/index5_page_1.html', 
                       'https://parsinger.ru/html/index5_page_2.html',
                       'https://parsinger.ru/html/index5_page_3.html', 
                       'https://parsinger.ru/html/index5_page_4.html'
                        ]
        ```
        
4. Асинхронная функция `async def main():`
    - В этой функции происходит создание session при помощи библиотеки **aiohttp**, также в этой функции мы создаем таски(задачи) `task = asyncio.create_task(get_data(session, link))`,в каждую задачу мы заворачиваем корутину `get_data(session, link)` и передаем в нее созданную ранее session и ссылку, полученную из глобального списка `pagen_lst`. Когда все задачи сформированы и список **tasks** наполнен, мы передаем распакованный список **tasks** в функцию `.gather(*tasks)`, чтобы она собрала все задачи и передала их в цикл событий. Каждый из тасков(задач) имеет тип  `<class '_asyncio.Task'>`. Каждый task является **awaitable**-объектом который подходит для их передачи в функцию `.gather(*tasks)` для дальнейшей отправки в цикл событий.
5. Функция `async def get_data(session, link):`
    
    - В этой функции происходит извлечение данных с каждой страницы. Эта функция получает на вход `session` и `link` из глобального списка со ссылками `pagen_lst=[]`.  В этой функции мы работаем с **BeautifulSoup** точно так же, как и работали бы при написании синхронного парсера. Здесь нам пришлось использовать `session` дважды, для более глубокого извлечения данных из карточек. Аналогичный код мы писали в задачах по сбору данных со всех 160 карточек с товаром. В асинхронном коде такой подход тоже допустим, т.к. мы используем одну `session` для всех наших запросов. Пример ниже наглядно продемонстрирует множественное переиспользование сессии для многоуровневого извлечения данных. Пример синтетический, но хорошо демонстрирует возможности глубины парсинга. Попробуйте понять логику этого кода, она не очень сложная, но понимание этого облегчит написание собственных парсеров.
        
        ```
        import aiohttp
        import asyncio
        from bs4 import BeautifulSoup
        
        
        async def main(url):
            async with aiohttp.ClientSession() as session:
                async with session.get(url=url) as response:
                    resp = await response.text()
                    soup = BeautifulSoup(resp, 'lxml')
                    item_card = [x['href'] for x in soup.find_all('a', class_='link')]
                    for url2 in item_card:
                        async with session.get(url=url2) as response2:
                            resp2 = await response2.text()
                            soup2 = BeautifulSoup(resp2, 'lxml')
                            item_card2 = [x['href'] for x in soup2.find_all('a', class_='link2')]
                            for url3 in item_card2:
                                async with session.get(url=url3) as response3:
                                    resp3 = await response3.text()
                                    soup3 = BeautifulSoup(resp3, 'lxml')
                                    item_card3 = [x['href'] for x in soup3.find_all('a', class_='link3')]
                                    for url4 in item_card3:
                                        async with session.get(url=url4) as response4:
                                            resp4 = await response4.text()
                                            soup4 = BeautifulSoup(resp4, 'lxml')
                                            data = [x['href'] for x in soup4.find_all('div', class_='data')]
                                            print(data)
        asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
        asyncio.run(main('http://example.com'))
        ```