К этому этапу курса вы уже умеете скачивать изображения при помощи bs4 и requests. В этом разделе мы рассмотрим скачивание 100 картинок и сравним скорость асинхронного и синхронного скачивания.

Для запуска этого кода создайте папку "images" для сохранения изображений. Все изображения будут скачаны с нашего [сайта](https://parsinger.ru/asyncio/aiofile/1/index.html).

```
import time
import aiofiles
import asyncio
import aiohttp
from bs4 import BeautifulSoup
import os


async def write_file(session, url, name_img):
    async with aiofiles.open(f'images/{name_img}', mode='wb') as f:
        async with session.get(url) as response:
            async for x in response.content.iter_chunked(1024):
                await f.write(x)
        print(f'Изображение сохранено {name_img}')


async def main():
    url = 'https://parsinger.ru/asyncio/aiofile/1/index.html'
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            soup = BeautifulSoup(await response.text(), 'lxml')
            img_url = [f'https://parsinger.ru/asyncio/aiofile/1/{x["src"]}' for x in soup.find_all('img')]
            tasks = []
            for link in img_url:
                name_img = link.split('/')[7]
                task = asyncio.create_task(write_file(session, link, name_img))
                tasks.append(task)
            await asyncio.gather(*tasks)


start = time.perf_counter()
asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
print(f'Cохранено изображений {len(os.listdir("images/"))} за {round(time.perf_counter() - start, 3)} сек')

#Результат
   Cохранено 100 изображений за 7.732 сек
```

В этом коде используются две корутины: `main()` и `write_file()`. Давайте вместе разберёмся, что делает каждая из них.

- Корутина `main()` является точкой входа. Именно её мы передаём для запуска всего асинхронного кода в цикл событий `asyncio.run(main())`. После её запуска происходит следующее:
    1. Инициализируется переменная `url` с указанием ссылки, куда будет отправлен запрос для дальнейшего извлечения изображения.
    2. Создаётся `ClientSession()` с применением диспетчера контекста для создания запросов через эту сессию и своевременного закрытия сессии.
    3. Используется метод `session.get(url)` с менеджером контекста для своевременного закрытия после получения ответа.
    4. Создаётся экземпляр супа `BeautifulSoup(await response.text(), 'lxml')`, в который мы передаём объект `response.text()` со стоящим рядом ключевым словом `await` для переключения контекста в цикле событий. Парсеры можно использовать любые; все они перечислены [тут](https://stepik.org/lesson/631856/step/1?unit=627882);
    5. Переменная `img_url` генерирует список ссылок, которые были извлечены из супа `soup.find_all('img')`. Каждая ссылка имеет окончательный вид, например, [https://parsinger.ru/asyncio/aiofile/1/img/166246153613344.jpg](https://parsinger.ru/asyncio/aiofile/1/img/166246153613344.jpg).
    6. Переменная `tasks = []` будет служить контейнером для наших задач, которые мы будем помещать в цикл событий.
    7. В цикле `for link in img_url:` мы делаем следующее:
        - `name_img = link.split('/')[7]`: извлекаем из каждой ссылки только её имя **[166246153613344](https://parsinger.ru/asyncio/aiofile/1/img/166246153613344.jpg)**, которое нам понадобится для именования каждого скачанного изображения.
            
        - `task = asyncio.create_task(write_file(session, link, name_img))` — функция `create_task()` оборачивает корутину в задачу (`task`) и планирует её выполнение. По этой причине список `tasks` будет наполнен запланированными к выполнению корутинами. Если мы посмотрим на тип объекта, создаваемого на каждой итерации цикла, то увидим, что его тип — `<class '_asyncio.Task'>`.
            
            - Кроме создания задач, в этой строке передаются данные, которые мы извлекли в текущей корутине, в корутину `write_file(session, link, name_img)`. А именно: открытую сессию, ссылку на изображение и имя изображения. Про эту корутину мы подробно поговорим ниже.
                
        - Когда список `tasks=[]` наполнен, мы распаковываем этот список и передаём его в цикл событий при помощи функции `await asyncio.gather(*tasks)`. Функция `.gather()` одновременно запускает все `awaitable`-объекты из списка `tasks=[]`.
            
- Корутина `write_file(session, url, name_img)` выполняется после основной корутины `main()` и занимается записью в файл объекта `response.content`, так как ссылки на изображения прямые и имеют формат .jpg. Также обратите внимание на асинхронный цикл `async for`: он выполняет асинхронное итерирование по байтовому содержимому файла. Получая объект и полный набор байт, он записывает в файл те части, которые обрабатывает в данную итерацию. Поскольку каждая картинка весит не более 1,5 МБ, файлы скачиваются практически целиком. Надеюсь, вы помните, что итерирование по файлу полезно, когда вы работаете с большими объектами. Также `.iter_chunked(1024)` можно удалить полностью, и скрипт продолжит работать медленнее примерно на одну-две секунды. Применять или не применять итерирование по файлу — решать вам. Но при скачивании огромного количества изображений эти одна-две секунды превратятся в целые минуты, а мы ведь хотим добиться высокой скорости работы наших парсеров.
    

Для сравнения этих двух подходов я подготовил синхронную версию кода, который делает то же самое, что и код выше. Для его запуска создайте папку `sync_save_files` в вашем проекте; туда будут сохранены все скачанные изображения.

```
import time
import requests
from bs4 import BeautifulSoup


def main(url):
    img_url = []
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'lxml')
    images_link = [f'https://parsinger.ru/asyncio/aiofile/1/{x["src"]}' for x in soup.find_all('img')]
    img_url.extend(images_link)

    for x in images_link:
        response2 = requests.get(x, stream=True).content
        name_img = x.split('/')[7]
        read_file = open(f'sync_save_files/{name_img}.jpg', 'wb')
        read_file.write(response2)


start = time.perf_counter()
url = 'https://parsinger.ru/asyncio/aiofile/1/index.html'
main(url)
print(time.perf_counter() - start)


#Результат
   61.421374800032936
```

Разница в коде впечатляет. Мы увеличили скорость скачивания практически в десять раз! Асинхронный код выглядит немного сложным и запутанным, но уверяю вас, это только на первый взгляд. Когда вы освоите асинхронное программирование, вы будете писать только асинхронные парсеры.