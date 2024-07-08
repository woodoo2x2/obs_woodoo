Давайте рассмотрим каждое исключение, генерируемое [[Модуль requests]] в Python, более подробно:

`requests**.RequestException**`: Это базовое исключение, от которого наследуются все другие исключения в библиотеке `requests`. Это амфиболическое исключение, которое может возникнуть во время обработки вашего запроса. Оно служит для перехвата всех исключений, генерируемых этой библиотекой.  
      
    **Частая причина возникновения**: Ошибки в обработке HTTP-запросов.
    
    ```
    import requests
    
    try:
        response = requests.get('https://nonexistentwebsite123456.com')
    except requests.RequestException as e:
        print('Произошла ошибка: ', str(e))
    ```
    
- requests**.`ConnectionError`**: Это исключение возникает, когда происходит ошибка соединения, например, когда не удается подключиться к серверу или потеряно соединение в процессе выполнения запроса.  
      
    **Частая причина возникновения**: Проблемы с соединением, такие как отсутствие интернета или недоступность сервера.
    
    ```
    import requests
    
    try:
        response = requests.get('https://nonexistentwebsite123456.com')
    except requests.ConnectionError as e:
        print('Ошибка соединения: ', str(e))
    ```
    
- `requests**.HTTPError**`: Это исключение генерируется, когда сервер возвращает ответ с HTTP-статусом, отличным от 2xx. Оно полезно для перехвата и обработки HTTP-ошибок.  
      
    **Частая причина возникновения**: Сервер возвращает код ошибки HTTP (например, 404 Not Found или 500 Internal Server Error).
    
    ```
    import requests
    
    try:
        response = requests.get('https://httpstat.us/500')
        response.raise_for_status()
    except requests.HTTPError as e:
        print('HTTP ошибка: ', str(e))
    ```
    
- `requests.**MissingSchema**`: Это исключение возникает, когда не предоставлен действительный URL для выполнения запроса. Другими словами, если URL отсутствует или недопустим, будет вызвано это исключение.  
      
    **Частая причина возникновения**: Недопустимый или отсутствующий URL.
    
    ```
    import requests
    
    try:
        response = requests.get('')
    except requests.exceptions.MissingSchema as e:
        print('Требуется URL с указанием схемы: ', str(e))
    ```
    
- `requests.**TooManyRedirects**`: Это исключение возникает, когда запрос перенаправляется слишком много раз. Это может быть результатом бесконечного редиректа или превышения максимального количества разрешенных редиректов.  
      
    **Частая причина возникновения**: Слишком много перенаправлений, возможно, из-за цикличного перенаправления.
    
    ```
    import requests
    
    try:
        response = requests.get('http://httpbin.org/relative-redirect/32', allow_redirects=True,)
    except requests.TooManyRedirects as e:
        print('Слишком много перенаправлений: ', str(e))
    ```
    
- `requests.**ConnectTimeout**`: Это исключение возникает, когда запрос не может быть выполнен из-за тайм-аута соединения с удаленным сервером. Запросы, которые вызвали это исключение, безопасно повторять.  
      
    **Частая причина возникновения**: Сервер не отвечает в течение установленного времени.
    
    ```
    import requests
    
    try:
        response = requests.get('https://httpstat.us/200', timeout=0.001)
    except requests.ConnectTimeout as e:
        print('Тайм-аут соединения: ', str(e))
    ```
    
- `requests.**ReadTimeout**`: Это исключение возникает, когда сервер не отправляет данные в отведенное количество времени. Это может произойти из-за проблем на стороне сервера или из-за недостаточно большого времени ожидания на стороне клиента.  
      
    **Частая причина возникновения**: Сервер не отправляет данные в отведенное время.
    
    ```
    import requests
    
    try:
        response = requests.get('https://httpstat.us/200?sleep=5000', timeout=1)
    except requests.ReadTimeout as e:
        print('Тайм-аут чтения: ', str(e))
    ```
    
- `requests.**Timeout**`: Это общее исключение для тайм-аутов, и его перехват будет перехватывать как `ConnectTimeout`, так и `ReadTimeout`. Это удобно, когда вы хотите обработать оба типа тайм-аутов одинаково.  
      
    **Частая причина возникновения**: Любой из вышеупомянутых тайм-аутов.
    
    ```
    import requests
    
    try:
        response = requests.get('https://httpstat.us/200?sleep=5000', timeout=1)
    except requests.Timeout as e:
        print('Тайм-аут запроса: ', str(e))
    ```
    
-` requests.**JSONDecodeError**:` Это исключение возникает, когда текст не может быть декодирован в JSON. Это обычно происходит, когда сервер возвращает ответ, который не является действительным JSON-объектом, но ожидалось, что он будет JSON.  
      
    **Частая причина возникновения**: Ответ сервера не может быть декодирован как JSON.
    
    ```
    import requests
    
    try:
        response = requests.get('https://httpstat.us/200')
        data = response.json()
    except requests.JSONDecodeError as e:
        print('Ошибка декодирования JSON: ', str(e))
    ```

#ПарсингPython 