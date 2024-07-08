Если ваша цель — обработать построчно один большой текстовый файл при помощи aiofiles, то, скорее всего, вам следует использовать привычный способ чтения, а именно синхронную функцию `open()`. Скорость будет несколько выше, чем при использовании асинхронной функции `open()`. Это происходит потому, что в `asyncio` многие методы являются "обертками" над  методами стандартных библиотек Python. Это с одной стороны позволяет поддерживать привычный синтаксис, а с другой стороны несколько усложняет выполнение тех же самых операций ради поддержки асинхронности. 

Давайте сравним работу методов на практике. Скачайте [file_1.txt](https://stepik.org/media/attachments/lesson/742823/file_1.txt), в котором 100 000 строк, и поместите его в папку с вашим проектом. Он понадобится для запуска кода, в котором мы будем сравнивать скорость его чтения в обычном и асинхронном стилях.

```
import time
import aiofiles
import asyncio

async_time = float()

def sync_read():
    start = time.perf_counter()
    with open(f'file_1.txt', 'r') as file:
        for line in file.readlines():
            print(line.strip())
    sync_time = round(time.perf_counter() - start, 5)
    return sync_time


async def async_read():
    start = time.perf_counter()
    async with aiofiles.open('file_1.txt', mode='r') as file:
        for line in await file.readlines():
            print(line.strip())
    async_time = round(time.perf_counter() - start, 5)
    return print(f"Файл считан построчно в асинхронном стиле за {async_time} сек")


sync = sync_read()
asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(async_read())
print(f"Файл считан построчно в синхронном стиле за {sync} сек")
```

Вывод:

```
Файл считан построчно в асинхронном стиле за 0.93855 сек
Файл считан построчно в синхронном стиле за 0.92342 сек
```

  
В этом примере мы видим, что чтение в синхронном стиле заняло немного меньше времени, чем в асинхронном. Различие незначительное, но оно есть.

В следующем примере мы создадим и построчно прочитаем сразу 100 000 текстовых файлов в каждом напишем  
"**Hello, world!**". Как думаете, какой подход победит по скорости?

```
import os
import time
import asyncio
import aiofiles

# Создание директории для файлов, если она не существует
os.makedirs('test_files', exist_ok=True)


# Создание 100000 текстовых файлов
def create_files():
    for i in range(1000):
        with open(f'test_files/file_{i}.txt', 'w') as f:
            f.write('Hello, world!\n' * 100)  # Запись некоторого количества строк для создания содержимого

create_files()  # Создание файлов

# Синхронное чтение файлов
def sync_read_files():
    start = time.perf_counter()
    for i in range(1000):
        with open(f'test_files/file_{i}.txt', 'r') as f:
            lines = f.readlines()
    sync_time = time.perf_counter() - start
    return sync_time


# Асинхронное чтение файлов
async def async_read_file(file_path):
    async with aiofiles.open(file_path, mode='r') as f:
        await f.readlines()


async def async_read_files():
    start = time.perf_counter()
    tasks = [async_read_file(f'test_files/file_{i}.txt') for i in range(1000)]
    await asyncio.gather(*tasks)
    async_time = time.perf_counter() - start
    return async_time


def main():
    sync_time = sync_read_files()                 # Синхронное чтение
    async_time = asyncio.run(async_read_files())  # Асинхронное чтение

    print(f"Синхронное чтение 1000 файлов заняло {sync_time:.5f} секунд")
    print(f"Асинхронное чтение 1000 файлов заняло {async_time:.5f} секунд")


if __name__ == '__main__':
    main()

```

Видим, что результат асинхронного подхода превзошёл синхронный в несколько раз. У вас финальные цифры могут отличаться; они зависят от скорости вашего железа. Какой же вывод можно сделать из этого? Вывод таков: нет смысла работать с одним файлом в асинхронном стиле. Асинхронность идеально подходит для работы с множественным вводом/выводом, и нужно чётко понимать и различать, где стоит её применять, а где нет. Такая же ситуация сохранится, если мы захотим скачать большое видео из интернета.

Конечно, такой код не может быть использован без цикла событий или в синхронной программе. Давайте изменим его, добавим в него всё необходимое и скачаем свой первый файл с помощью **aiofiles**.

Перед запуском этого кода создайте папку `video`, в которую будет сохранён скачанный файл.

```
import aiofiles
import asyncio
import aiohttp

async def async_write(url):
    async with aiohttp.ClientSession() as session:

        async with aiofiles.open('video/async_video_async.mp4', mode='wb') as video:

            async with session.get(url) as response:
                async for piece in response.content.iter_chunked(5120):
                    await video.write(piece)


url = 'https://parsinger.ru/asyncio/aiofile/1/video/nu_pogodi.mp4'
asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(async_write(url))
```