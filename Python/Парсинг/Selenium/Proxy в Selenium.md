# Proxy и Selenium

Работа с прокси в Selenium очень проста, намного проще, чем в [requests](https://stepik.org/lesson/691440/step/4?unit=690987), где мы создавали словарь, прописывали в ключах схемы, и затем передавали его в запросе.

Мы можем узнать свой **IP** на сайте [2ip.ru](https://2ip.ru/). Выполните код ниже, чтобы увидеть к принт в консоли с  вашим **IP** адресом.

```python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By

url = 'https://2ip.ru/'
with webdriver.Chrome() as browser:
    browser.get(url)
    time.sleep(5)
    print(browser.find_element(By.ID, 'd_clip_button').find_element(By.TAG_NAME, 'span').text)
    time.sleep(5)

>>> 95.27.00.01
```

Теперь модифицируем данный код, чтобы запрос отправлялся через прокси. 

Прокси должен быть вида **IP**:**PORT**

```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By

proxy = '8.210.83.33:80'
url = 'https://2ip.ru/'

chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--proxy-server=%s' % proxy)

with webdriver.Chrome(options=chrome_options) as browser:
    browser.get(url)
    print(browser.find_element(By.ID, 'd_clip_button').find_element(By.TAG_NAME, 'span').text)
    time.sleep(5)
```

Посмотрите внимательно на код и определите отличия между этими двумя примерами. 

Первое, что мы сделали, - передали параметр `'--proxy-server=%s' % proxy` методу `.add_argument()` в класс дополнительных опций .`ChromeOptions()` и передали сам прокси, который лежал в переменной proxy. Если этот прокси еще живой, можете запустить этот код у себя в **IDE**. Если прокси умер, то приобретите [надёжный прокси](https://proxy6.net/?r=408871), и запустите код с ним.

Подобно [requests](https://stepik.org/lesson/691440/step/4?unit=690987),  мы можем установить **timeout=** для загрузки страницы, после истечению которого произойдет, либо закрытие окна, либо переход к следующему прокси.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

proxy_list = ['8.210.83.33:80', '199.60.103.28:80', 
'103.151.246.38:10001', '199.60.103.228:80', 
'199.60.103.228:80', '199.60.103.28:80', ]

for PROXY in proxy_list:
    try:
        chrome_options = webdriver.ChromeOptions()
        chrome_options.add_argument('--proxy-server=%s' % PROXY)
        url = 'https://2ip.ru/'

        with webdriver.Chrome(options=chrome_options) as browser:
            browser.get(url)
            print(browser.find_element(By.ID, 'd_clip_button').find_element(By.TAG_NAME, 'span').text)

            browser.set_page_load_timeout(5)

            proxy_list.remove(PROXY)
    except Exception as _ex:
        print(f"Превышен timeout ожидания для - {PROXY}")
        continue
```

В этом примере, есть список прокси `proxy_list`, по которому мы итерируемая в цикле `for`, передавая на каждой итерации в переменную `PROXY`, следующий **IP** из этого списка. Мы применили конструкцию **try/except**, чтобы наш скрипт не падал с ошибкой, и продолжал работать. Если не обернуть код в **try/excep**t, мы после каждого истекшего **timeout=** мы будем получать ошибку:  `Message: unknown error: net::ERR_TUNNEL_CONNECTION_FAILED`.

**timeout** в Selenium применяется методом **.set_page_load_timeout(5)** где цифра 5 длительность в секундах.

p.s. Если вы найдёте работающий прокси, то этот код напечатает вам его в консоль = )

## Proxy с авторизацией

Для настройки прокси с авторизацией вам потребуется отдельно установить расширение **seleniumwire.**

Делается это так:

```
Установка

pip install selenium-wire


Импорт

from seleniumwire import webdriver
```

Если у вас есть рабочий прокси, используйте его, или приобретите за 100 р/шт в магазине [https://proxy6.net/](https://proxy6.net/?r=408871), для запуска следующего кода(_прокси в примере ниже, может не работать_).

```
import time
from selenium.webdriver.common.by import By
from seleniumwire import webdriver

options = {'proxy': {
    'http': "socks5://D2Frs6:75JjrW@194.28.210.39:9867",
    'https': "socks5://D2Frs6:75JjrW@194.28.210.39:9867",
    }}

url = 'https://2ip.ru/'

with webdriver.Chrome(seleniumwire_options=options) as browser:
    browser.get(url)
    print(browser.find_element(By.ID, 'd_clip_button').find_element(By.TAG_NAME, 'span').text)
    time.sleep(5)
```

Как вы можете заметить, структура кода почти не изменилась:  мы использовали `options=options`, а теперь используем `seleniumwire_options=options`, в словаре `options`, лежит прокси с авторизацией, и нам не нужно использовать метод добавления аргумента `.add_argument()`.

Когда вы покупаете прокси в магазине, они могут имеет два вида "авторизации: по логину и паролю и по вашему IP адресу; вы можете выбрать удобный для вас способ. Если ваш провайдер выдает динамический IP, который меняется при каждой перезагрузке ПК или роутера, то лучше выбрать авторизацию по логину и паролю, если IP статический, то лучше выбрать авторизацию по IP.

В магазине [proxy6.net](https://proxy6.net/?r=408871) это делается в один клик.

![](https://ucarecdn.com/5142a362-681d-499f-8bf7-443e55e2ed9f/)![](https://ucarecdn.com/17910003-493f-4d1b-acd3-1bc62945362f/)