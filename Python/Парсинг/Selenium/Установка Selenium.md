# Простая установка WebDriver с помощью Webdriver Manager

Основная идея [Webdriver Manager](https://pypi.org/project/webdriver-manager/) — упростить установку драйверами для разных браузеров.

К основным преимуществам использования `webdriver_manager` в парсерах на заказ имеет несколько ключевых факторов, когда дело доходит до установки и обновления WebDriver на компьютере заказчика:

- **Удобство для клиента**: Клиентам не нужно заботиться о технических деталях установки или обновления WebDriver. Это особенно полезно для тех, кто не имеет технических навыков.
- **Экономия времени**: Не потребуется времени на объяснение клиентам процесса установки или обновления, что ускоряет процесс разработки и реализации.
- **Меньше ошибок**: Автоматическое управление драйверами уменьшает вероятность ошибок из-за несоответствия версий или неправильной установки.
- **Поддержка нескольких браузеров**: `webdriver_manager` поддерживает различные драйверы (Chrome, Firefox и т.д.), что делает процесс адаптации парсера к разным браузерам гораздо проще.
- **Обновления "на лету"**: Даже если во время использования парсера выходит новая версия браузера, `webdriver_manager` автоматически скачает соответствующую версию драйвера.

В данном степе мы рассмотрим, как автоматически установить `WebDriver` только для Chrome.

_Установка `WebDriver` для других браузеров подробно рассмотрена в  официальной документацией. Прямые ссылки на инструкции для каждого браузера предоставлены ниже._

- [ChromeDriver](https://pypi.org/project/webdriver-manager/#use-with-chrome)
- [EdgeChromiumDriver](https://pypi.org/project/webdriver-manager/#use-with-edge)
- [GeckoDriver](https://pypi.org/project/webdriver-manager/#use-with-firefox)
- [IEDriver](https://pypi.org/project/webdriver-manager/#use-with-ie)
- [OperaDriver](https://pypi.org/project/webdriver-manager/#use-with-opera)

Приступим, сначала установите библиотеку Selenium, если это еще не сделано:

```
pip install selenium
```

Далее, установите пакет `webdriver**_**manager`, если он еще не установлен:

```
pip install webdriver-manager
```

Теперь вы можете запустить следующий код, после чего запустится Chrome и загрузится страница нашего курса:

```python
import time
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager

with webdriver.Chrome(ChromeDriverManager().install()) as driver:
    driver.get("https://stepik.org/course/104774")
    time.sleep(5)

# Или

import time
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from webdriver_manager.core.os_manager import ChromeType
from selenium.webdriver.chrome.service import Service as ChromiumService

with webdriver.Chrome(
        service=ChromiumService(ChromeDriverManager(chrome_type=ChromeType.CHROMIUM).install())) as driver:
    driver.get("https://stepik.org/course/104774")
    time.sleep(5)
```

Когда интерпретатор дойдёт до строки `ChromeDriverManager(chrome_type=ChromeType.CHROMIUM).install()`, будет происходить следующее:

1. Библиотека `webdriver_manager` проверит, установлен ли уже на вашем компьютере нужный `chromedriver` и соответствует ли он версии вашего браузера Chrome. Импорт `ChromeType` используется только в случаях, когда требуется указать специфический тип браузера. В остальных случаях его можно не использовать.
    
2. Если нужный `chromedriver` не найден или его версия не соответствует, `webdriver_manager` автоматически скачает нужную версию ChromeDriver с официального сайта.
    
3. Загруженный файл будет распакован и сохранён в удобное место (обычно это каталог `.cache` в вашем домашнем каталоге).
    
4. Путь к этому распакованному файлу `chromedriver` будет автоматически добавлен к вашей сессии, и WebDriver будет инициализирован с использованием этого драйвера.
    

В результате, вам не нужно вручную загружать, распаковывать и настраивать путь к `chromedriver`. Всё это делает `webdriver_manager` автоматически.

`webdriver_manager` загружает некоторые веб-драйверы из своих официальных репозиториев GitHub, но GitHub имеет ограничения, такие как 60 запросов в час для не аутентифицированных пользователей. Чтобы не столкнуться с ошибкой, связанной с учетными данными GitHub, вам необходимо создать токен GitHub и поместить его в свою среду: (*).