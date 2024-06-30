# Передача параметров в URL params=

В работе с сайтами часто возникает необходимость передачи определённых данных в строке запроса URL. Если вы формируете URL вручную, то эти параметры обычно указываются в виде пар ключ/значение после знака вопроса. Например, `httpbin.org/get?key=val`. Библиотека Requests предоставляет возможность передачи этих параметров в виде словаря, используя аргумент `params=`. Допустим, вы хотите передать параметры `key1=value1` и `key2=value2` ресурсу `httpbin.org/get`. В этом случае код будет выглядеть следующим образом:

```python
# Создание словаря с параметрами
params = {'key1': 'value1', 'key2': 'value2'}

# Отправка GET-запроса с параметрами
r = requests.get('https://httpbin.org/get', params=params)

# Вывод результирующего URL
print(r.url)

>>> https://httpbin.org/get?key1=value1&key2=value2
```

 Как видно, URL был сформирован правильно:

```
https://httpbin.org/get?key1=value1&key2=value2
```

### Что такое `params=` и для чего он используется? 

Аргумент `params=` — это функциональность библиотеки Requests, которая позволяет передавать параметры в URL в виде словаря Python. Эти параметры автоматически конвертируются и добавляются к базовому URL, формируя таким образом полный URL запроса.

```python
# Импорт библиотеки
import requests

# Параметры запроса: ищем книги по программированию
params = {'text': 'WEB Парсинг на python'}

# Отправка запроса
response = requests.get('https://yandex.ru/search/', params=params)

# Вывод результатов
print(response.text)
```

Сформированная ссылка куда будет отправлен запрос будет выглядеть так:

```vbscript
https://yandex.ru/search/?text=WEB Парсинг на python
```

Здесь `https://yandex.ru/search/` — это базовый URL, к которому добавляются параметр text с их соответствующим значением (`WEB Парсинг на python)`.

## Когда использовать `params=`?

1. **Работа с API**: В большинстве случаев API требуют передачи определенных параметров для доступа к нужным данным.
    
    ```
    import requests
    
    # Ваш API-ключ от OpenWeather (замените на реальный ключ)
    api_key = "ВАШ_API_КЛЮЧ"
    
    # Город, для которого мы хотим получить погодные данные
    city = "Москва"
    
    # Словарь параметров для передачи в API
    params = {
        'q': city,        # Название города
        'appid': api_key, # Ваш API-ключ
        'units': 'metric' # Единицы измерения (опционально)
    }
    
    # Базовый URL API сервиса погоды
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    
    # Отправляем GET-запрос к API
    response = requests.get(base_url, params=params)
    
    # Проверяем статус ответа
    if response.status_code == 200:
        # Выводим полученные данные о погоде
        print("Погодные данные для города {}: ".format(city))
        print(response.json())
    else:
        # Выводим сообщение об ошибке
        print("Не удалось получить данные. Код ошибки: {}".format(response.status_code))
    ```
    
    Cформированная ссылка куда будет отправлен запрос будет выглядеть так:  
     `**http://api.openweathermap.org/data/2.5/weather?q=Москва&appid=ВАШ_API_КЛЮЧ&units=metric**`  
     
    
2. **Динамические веб-сайты**: Для работы с динамическими веб-сайтами, где контент зависит от определённых переменных, параметры URL играют ключевую роль. Рассмотрим пример, в котором мы хотим парсить результаты поиска с некоторого фиктивного онлайн-магазина электроники. В этом примере мы будем использовать библиотеку Requests для отправки GET-запроса с параметрами, задающими фильтры для поиска.
    
    ```
    import requests
    from bs4 import BeautifulSoup
    
    # Базовый URL для поиска в онлайн-магазине
    base_url = 'https://example.com/search'
    
    # Параметры поиска: ищем ноутбуки с определенными характеристиками
    search_params = {
        'query': 'note',  # Поисковый запрос
        'brand': 'Dell',  # Бренд
        'min_price': '50000',  # Минимальная цена
        'max_price': '100000',  # Максимальная цена
        'sort': 'price_asc'  # Сортировка по возрастанию цены
    }
    
    # Отправляем GET-запрос с параметрами
    response = requests.get(base_url, params=search_params)
    
    # Проверка успешности запроса
    if response.status_code == 200:
        # Парсим HTML-страницу с использованием BeautifulSoup
        soup = BeautifulSoup(response.text, 'html.parser')
    
        # Находим все элементы, соответствующие карточкам товаров (здесь это примерный CSS-селектор)
        product_cards = soup.select('.product-card')
    
        for card in product_cards:
            # Извлекаем информацию из карточки товара (название и цена)
            product_name = card.select_one('.product-name').text
            product_price = card.select_one('.product-price').text
    
            print(f'Название товара: {product_name}')
            print(f'Цена товара: {product_price}')
            print('---')
    else:
        print(f'Не удалось выполнить запрос. Код ошибки: {response.status_code}')
        print(response.url)
    ```
    
     Cформированная ссылка куда будет отправлен запрос будет выглядеть так:   
    `**https://example.com/search?query=note&brand=Dell&min_price=50000&max_price=100000&sort=price_asc**`  
     
    
3. **Пагинация**: При работе с веб-сайтами с пагинацией (разбиением контента на несколько страниц) параметры URL часто используются для указания конкретной страницы, с которой нужно собрать данные. Возьмём для примера фиктивный веб-сайт новостей, где каждая страница содержит 10 статей.
    
    ```
    import requests
    from bs4 import BeautifulSoup
    
    # Базовый URL веб-сайта новостей
    base_url = 'https://example.com/news'
    
    # Номер страницы, с которой мы хотим собрать данные
    page_number = 2
    
    # Словарь параметров для передачи в запрос
    params = {
        'page': page_number  # Параметр для указания номера страницы
    }
    
    # Отправляем GET-запрос с параметрами
    response = requests.get(base_url, params=params)
    
    # Проверка успешности запроса
    if response.status_code == 200:
        # Парсим HTML-страницу с использованием BeautifulSoup
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Находим все элементы, соответствующие новостным статьям (это примерный CSS-селектор)
        news_articles = soup.select('.news-article')
        
        for article in news_articles:
            # Извлекаем заголовок каждой статьи
            article_title = article.select_one('.article-title').text
            print(f'Заголовок статьи: {article_title}')
    else:
        print(f'Не удалось выполнить запрос. Код ошибки: {response.status_code}')
    ```
    
    Cформированная ссылка куда будет отправлен запрос будет выглядеть так:   
    **`https://example.com/news?page=2`**  
    Параметр `page` указывает на номер страницы, данные с которой мы хотим собрать.  
      
    Вы можете использовать цикл `for` для изменения номера страницы в параметрах запроса.  
    Вот простой пример кода:
    
    ```
    import requests
    from bs4 import BeautifulSoup
    
    # Базовый URL веб-сайта новостей
    base_url = 'https://example.com/news'
    
    # Перебираем страницы от 1 до 10
    for page_number in range(1, 11):
    
        # Словарь параметров для передачи в запрос
        params = {
            'page': page_number  # Параметр для указания номера страницы
        }
    
        # Отправляем GET-запрос с параметрами
        response = requests.get(base_url, params=params)
    
        print(response.url)
    
       
    ```
    
    Вывод:
    
    ```
    # Запросы будут отправлены на эти ссылки
    
    https://example.com/news?page=1
    https://example.com/news?page=2
    https://example.com/news?page=3
    https://example.com/news?page=4
    https://example.com/news?page=5
    https://example.com/news?page=6
    https://example.com/news?page=7
    https://example.com/news?page=8
    https://example.com/news?page=9
    https://example.com/news?page=10
    ```
    
    Цикл `for` от 1 до 10 (`for page_number in range(1, 11)`) используется для перебора номеров страниц.  
      
    Внутри цикла параметр `page` обновляется на каждой итерации, что позволяет передавать номер текущей страницы в GET-запросе.  
      
    После получения и обработки данных каждой страницы код выводит информацию о том, с какой страницы были собраны данные, а также заголовки статей с этой страницы.
    
4. **Фильтрация данных**: Для демонстрации фильтрации данных рассмотрим пример с фиктивным API, предоставляющим информацию о фильмах. Предположим, что мы хотим получить список фильмов, выпущенных после определенного года и принадлежащих определенному жанру.
    
    ```
    import requests
    
    # Базовый URL для API фильмов
    base_url = 'https://api.example.com/movies'
    
    # Параметры для фильтрации фильмов
    params = {
        'year_after': 2000,  # Фильмы, выпущенные после этого года
        'genre': 'action'    # Жанр фильма
    }
    
    # Отправляем GET-запрос с параметрами для фильтрации
    response = requests.get(base_url, params=params)
    
    # Проверка успешности запроса
    if response.status_code == 200:
        # Выводим полученные данные о фильмах
        print("Список фильмов:")
        for movie in response.json():
            print(f'  Название: {movie["title"]}, Год выпуска: {movie["year"]}, Жанр: {movie["genre"]}')
    else:
        # Выводим сообщение об ошибке
        print(f'Не удалось выполнить запрос. Код ошибки: {response.status_code}')
    ```
    
     Как будет выглядеть сформированная ссылка:  
    **`https://api.example.com/movies?year_after=2000&genre=action`**  
      
    `year_after=2000`: Параметр `year_after` указывает на то, что нам нужны фильмы, выпущенные после 2000 года.  
    `genre=action`: Параметр `genre` указывает, что нам нужны фильмы в жанре "action" (боевик).