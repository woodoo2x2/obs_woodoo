Aiofiles не умеет творить чудеса; она всего лишь работает асинхронно с вашими файлами. Если вы захотите скачать один большой видеофайл из интернета, вы не добьётесь большей скорости, чем при обычном скачивании. Чтобы продемонстрировать это, я загрузил на хостинг мультфильм "Ну, погоди!", который весит 212 МБ.

Код ниже скачает видеофайл размером 212 МБ дважды: сначала в синхронном стиле, а затем в асинхронном. После завершения скачивания он напечатает время, затраченное на скачивание. Создайте папку "video" в своём проекте для сохранения видеофайла и запустите код ниже. Скорость скачивания будет зависеть от скорости вашего интернет-соединения.

```
import requests
import time
import aiofiles
import asyncio
import aiohttp

url = 'https://parsinger.ru/asyncio/aiofile/1/video/nu_pogodi.mp4'


def sync_write():
    with open('video/sync_video_async.mp4', 'wb') as video:
        response = requests.get(url, stream=True)
        for piece in response.iter_content(chunk_size=5120):
            video.write(piece)


async def async_write():
    async with aiohttp.ClientSession() as session:
        async with aiofiles.open('video/async_video_async.mp4', mode='wb') as video:
            async with session.get(url) as response:
                async for piece in response.content.iter_chunked(5120):
                    await video.write(piece)


start = time.perf_counter()
sync_write()
print(f'Cохранено синхронным способом за {round(time.perf_counter() - start, 3)} сек')

start = time.perf_counter()
asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(async_write())
print(f'Cохранено асинхронным способом за {round(time.perf_counter() - start, 3)} сек')


#Результат
    Cохранено синхронным способом за 60.593 сек
    Cохранено асинхронным способом за 58.765 сек
```