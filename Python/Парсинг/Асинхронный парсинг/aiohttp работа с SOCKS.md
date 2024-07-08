# Aiohttp работа с SOCKS

В aiohttp работа с SOCKS отличается от работы с простыми HTTP/HTTPS-прокси, и в этом разделе мы подробно об этом поговорим.

Для работы с SOCKS4 и SOCKS5 нам необходимо использовать класс `ProxyConnector` из модуля `aiohttp_socks`. Для этого его нужно импортировать. Заодно импортируем `ProxyType` и `ChainProxyConnector`. Они немного сложнее в работе и для написания парсера с множеством прокси подходят не совсем хорошо, но знать про них нужно.

```
from aiohttp_socks import ProxyType, ProxyConnector, ChainProxyConnector
```

### **ProxyConnector** 

**ProxyConnector** - это самый простой способ передать SOCKS4 и SOCKS5 в асинхронную сессию, для этого нужно передать прокси как продемонстрировано в коде ниже. 

```
async def main(url):

    connector = ProxyConnector.from_url('socks5://user:password@127.0.0.1:1080')

    async with aiohttp.ClientSession(connector=connector, trust_env=True) as session:
        async with session.get(url) as response:
            return await response.text()
```

Мы можем использовать конструктор и настроить параметры нашего прокси, такой вариант удобен, если вы используете один прокси для ваших запросов. Как правило, для написания парсеров использовать конструктор не обязательно.

```
connector = ProxyConnector(
    proxy_type=ProxyType.SOCKS5,
    host='127.0.0.1',
    port=1080,
    username='user',
    password='password',
    rdns=True
)
```

Самая распространенная ситуация — когда у вас есть список рабочих прокси. Вы хотите поочередно передать каждый прокси в сессию, тем самым увеличивая время жизни вашего парсера. Для этого нам понадобится код с прошлого раздела, который мы немного изменили.

Для запуска этого кода вам потребуется файл `proxy_list_SOCKS.txt`, который вам необходимо создать самостоятельно. Поместите свои SOCKS в этот файл. Обратите внимание, что мы передаем в сессию только **`connector`**, в котором на каждой итерации цикла будет храниться новый SOCKS.

Этот код создает на каждой итерации новую `ClientSession()`, в которую мы передаем параметры `ProxyConnector`, в котором на каждой итерации мы передаем новый прокси. Пересоздавать на каждой итерации новую сессию считается не очень хорошей идеей, но для небольших парсеров вполне сгодится. Сильная сторона такого подхода в том, что при плохом прокси в списке код продолжит работать со следующим, и так до тех пор, пока прокси не закончатся. Как делать повторные запросы через прокси, которые были забанены, мы поговорим в следующем разделе курса.

```
import aiohttp
import asyncio
import aiofiles
from aiohttp_socks import ProxyConnector


async def main():
    async with aiofiles.open('proxy_list_SOCKS.txt', mode='r') as f:
        for prx in await f.readlines():
            url = 'http://httpbin.org/ip'
            connector = ProxyConnector.from_url(f'socks4://{prx}')
            try:
                async with aiohttp.ClientSession(connector=connector, timeout=.5) as session:
                    async with session.get(url=url, timeout=1) as response:
                        if response.status:
                            print(f'good proxy, status_code -{response.status}-', prx, end='')
            except Exception as _ex:
                continue


asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
```

### ChainProxyConnector

**ChainProxyConnector** полезен тем, что позволяет использовать сразу большой список прокси. Ваш список прокси может быть любой длины, и вы можете поместить его в память программы, записав в переменную. Однако это не совсем хорошая практика, потому что при большом количестве прокси ваш код будет выглядеть ужасно. Метод `from_urls([list])` принимает на вход список, который может быть извлечён напрямую из файла. В следующих примерах мы рассмотрим, как это сделать.

```
#Передаём список с прокси напрямую в from_urls()
connector = ChainProxyConnector.from_urls([
    'socks5://user:password@127.0.0.1:1080',
    'socks4://127.0.0.1:1081',
    'http://user:password@127.0.0.1:3128',
])

async with aiohttp.ClientSession(connector=connector) as session:
    async with session.get(url) as response:
        return await response.text()
```

У такого подхода есть одна очень важная особенность, которая портит всю картину. Вам нужно указывать схему, по которой работают ваши прокси: `socks4://`, `socks5://` или `http://`. Прокси `socks4` не будут работать по схеме `socks5`. По этой причине нужно заранее знать, какие прокси вы используете. Для парсинга хорошо подходят `socks4` и `socks5`.

В этом коде мы открываем файл **SOCKS5.txt,** который содержит прокси вида.

![](https://ucarecdn.com/cfcf762a-f0b1-42ad-9ff2-ca3df4a25687/)

Читаем построчно прокси из файла, обрабатывая каждую и добавляя к ней схему, по которой она работает, формируем список **proxy** для передачи в **ChainProxyConnector**. 

```
import aiohttp
import asyncio
import aiofiles
from aiohttp_socks import ChainProxyConnector


async def main():
    url = 'http://httpbin.org/ip'
    proxy = []
    async with aiofiles.open('SOCKS5.txt', mode='r') as socks4:
        for s4 in await socks4.readlines():
            proxy.append(f'socks5://{s4}')

    connector = ChainProxyConnector.from_urls(proxy)
    async with aiohttp.ClientSession(connector=connector) as session:
        async with session.get(url=url) as response:
            if response.status:
                print(f'good proxy, status_code -{response.status}-', s4, end='\n')
            elif response.status >= 400:
                print(f'bad proxy, status_code -{response.status}-', s4, end='\n')


asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
```

Ниже, тот же самый код, только теперь он берет прокси непосредственно из списка `proxy = [].` Такой код будет работать, если все прокси в списке живые, этот код упадет при первом нерабочем прокси и вы получите ошибку =>

```
[Errno 11001] Could not connect to proxy 194.28.210.392:9867 [getaddrinfo failed]
```

```
import aiohttp
import asyncio
from aiohttp_socks import ChainProxyConnector


async def main():
    url = 'http://httpbin.org/ip'
    proxy = [
       'socks5://D2Frs6:75JjrW@194.28.210.39:9867',
       'socks5://D2Frs6:75JjrW@194.28.209.68:9925'
    ]
    connector = ChainProxyConnector.from_urls(proxy)
    try:
        async with aiohttp.ClientSession(connector=connector, timeout=.5, trust_env=True) as session:
            async with session.get(url=url, timeout=1) as response:
                if response.status:
                    print(f'good proxy, status_code -{response.status}-', end='')
                elif response.status >= 400:
                    print(f'bad proxy, status_code -{response.status}-', end='')
    except Exception as _ex:
        print(_ex)


asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
```

### **ProxyType** 

**ProxyType** - используется для указания типа прокси, что в практике используется редко, т.к **ChainProxyConnector** проще и удобнее в использовании.

```
    connector = ProxyConnector(
    proxy_type=ProxyType.SOCKS5,
    host='127.0.0.1',
    port=1080,
    username='user',
    password='password',
    rdns=True
    )
    async with aiohttp.ClientSession(connector=connector) as session:
        async with session.get(url) as response:
            return await response.text()
```

Код ниже выполняет ту же самую задачу, что и код выше, только теперь у нас есть специальные поля для параметров прокси. В **ProxyType** появилось поле **rdns**(Reverse DNS), подробнее про него можно почитать на  [wiki](https://en.wikipedia.org/wiki/Reverse_DNS_lookup), на практике мне никогда не приходилось работать с этим параметром. Ради эксперимента я проверил прокси, купленные на  [proxy6.net](https://proxy6.net/?r=408871), они работают с `rdns=True` и `rdns=False`.

```
import aiohttp
import asyncio
from aiohttp_socks import ProxyConnector, ProxyType


async def main():
    url = 'http://httpbin.org/ip'
    connector = ProxyConnector(
            proxy_type=ProxyType.SOCKS5,
            host='194.28.210.39',
            port=9867,
            username='D2Frs6',
            password='75JjrW',
            rdns=True
            )

    async with aiohttp.ClientSession(connector=connector, timeout=.5, trust_env=True) as session:
        async with session.get(url=url, timeout=1) as response:
            if response.status:
                print(f'good proxy, status_code -{response.status}-', end='')
            elif response.status >= 400:
                print(f'bad proxy, status_code -{response.status}-', end='')


asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
```