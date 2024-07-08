## Прокрутка содержимого страницы - `execute_script()`

Полоса прокрутки представляет собой тонкую, длинную часть на краю дисплея компьютера. Используя полосу прокрутки, мы можем просматривать весь контент или всю страницу, прокручивая её вверх и вниз или влево и вправо с помощью мыши.

Самый простой способ прокрутки страницы по пикселям — это использование метода `.execute_script()`, который выполняет код JavaScript на странице. К примеру, `window.scrollBy(0, 5000)` прокрутит страницу вниз на 5000 пикселей.

Можете проверить на этом [сайте](http://parsinger.ru/scroll/1/)

`window.scrollBy(_X_, _Y_);`

- `X` - смещение в пикселях по горизонтали;
- `Y` - смещение в пикселях по вертикали.

```python
import time
from selenium import webdriver

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    browser.execute_script("window.scrollBy(0,5000)")
    time.sleep(10)
```

Такой способ имеет свои преимущества, простота использования — одно из них. Недостаток такого способа заключается в том, что если сайт отдаёт данные при каждом скроллинге, вам придётся ждать, пока сервер загрузит данные. К примеру, на Степике комментарии загружаются по 17 штук, и если под степом 170 комментариев, то вам придётся сделать 10 скроллов, чтобы получить их все. Каждая загрузка 17 комментариев занимает примерно 2–3 секунды, соответственно, вам необходимо устанавливать тайминги. Самый простой способ сделать это — `time.sleep(3)`.

Напишем простой код, который прокрутит страницу в низ за 10 итераций.

```python
import time
from selenium import webdriver

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    for i in range(10):
        browser.execute_script("window.scrollBy(0,5000)")
        time.sleep(2)
```

Мы сделали 10 итераций и не оказались в самом низу страницы, потому что не знаем, какова высота нашего сайта в пикселях. Мы можем использовать большие значения, например, `window.scrollBy(0, **500000**)`, чтобы одним скроллингом прокрутить всю страницу. Это подходит, когда у вас обычный сайт без динамической подгрузки данных.

Представим, что у вас есть сайт, который имеет разные высоты страниц. Мы можем получить значение высоты непосредственно той части сайта, которая попадает в область вашей видимости, или значение высоты всего сайта.

Команда `return document.body.scrollHeight` вернёт значение высоты основного элемента на странице — `body`.

```python
import time
from selenium import webdriver

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    height = browser.execute_script("return document.body.scrollHeight")
    time.sleep(2)
    print(height)

>>>81000
```

Наш сайт имеет высоту **81 000** пикселей. Для вычисления высоты видимой области сайта используется скрипт.

Код `window.innerHeight` используется для получения высоты, а `window.innerWidth` — для получения ширины видимой области.

```python
from selenium import webdriver

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    height = browser.execute_script("return window.innerHeight")
    print(height)

>>> 887
```

На нашем сайте видимая область составляет **887** пикселей. Определённые методы, такие как `.click()` или `.send_keys()`и др., могут быть выполнены только в случае, если нужный элемент расположен в этой видимой зоне экрана.

Если вам необходимо прокрутить страницу до самого низа, до последнего пикселя, одним из самых простых способов будет использование скрипта `window.scrollTo(0, document.body.scrollHeight)`.

```python
import time
from selenium import webdriver

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    browser.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    time.sleep(2)
```

Где значение `0` означает горизонтальное смещение в пикселях от начальной точки прокрутки. В данном случае, `0` говорит о том, что прокрутка будет совершена без горизонтального смещения, то есть по вертикали.

При написании парсеров часто необходимо сначала совершить необходимое количество скроллинга, чтобы загрузилась вся нужная вам информация. После того, как вся необходимая информация появилась на странице, мы собираем всё при помощи метода `.find_element**s**()`. Однако об этом мы будем говорить позже.


## `.execute_script()`

Синтаксис: `webdriver.execute_script**(script, *args)**`**.**

В методе `.execute_script()` можно использовать различные полезные параметры. Полный список всех событий можно просмотреть [тут](https://developer.mozilla.org/ru/docs/Web/API/Document) и [тут](https://developer.mozilla.org/ru/docs/Web/API/Window), но ниже приведены те, которые чаще всего используются при написании парсеров:

- `.execute_script(**"return arguments[0].scrollIntoView(true);"**, **element**)` — прокручивает родительский контейнер элемента таким образом, чтобы `element`, для которого вызывается `scrollIntoView`, был виден пользователю.
    
- `.execute_script("window.open(**'http://parsinger.ru'**, **'tab2'**);")` — создаст новую вкладку в браузере с именем "tab2".
    
- `.execute_script(**"return document.body.scrollHeight"**)` — вернёт значение высоты элемента `<body>`.
    
- `.execute_script(**"return window.innerHeight"**)` — вернёт значение высоты окна браузера.
    
- `.execute_script(**"return window.innerWidth"**)` — вернёт значение ширины окна браузера.
    
- `.execute_script(**"window.scrollBy(X, Y)"**)` — прокрутит документ на заданное число пикселей по осям X и Y.
    
    - `X` — смещение в пикселях по горизонтали.
    - `Y` — смещение в пикселях по вертикали.
- `.execute_script(**"alert(****'Ура Selenium')"**)` — вызывает модальное окно Alert.
    
- `.execute_script(**"return document.title;"**)` — возвращает `title` открытого документа.
    
- `.execute_script(**"return document.documentURI;"**)` — возвращает URL документа.
    
- `.execute_script(**"return document.readyState;"**)` — возвращает состояние загрузки страницы; вернёт "complete", если страница загрузилась.
    
- `.execute_script(**"return document.anchors;"**)` — возвращает список всех [якорей](http://htmlbook.ru/samhtml/yakorya) на странице.
    
    - `[_x.tag_name for x in browser.execute_script(**"return document.anchors;"**)_]` — этот код позволяет получить список всех тегов с якорями. Очень полезная инструкция, особенно если при скроллинге элемент для "зацепления" не найден.
- `.execute_script(**"return document.cookie;"**)` — возвращает строку, содержащую все cookies документа, разделённые точкой с запятой.
    
- `.execute_script(**"return document.domain;"**)` — возвращает домен текущего документа.
    
- `.execute_script(**"return document.forms;"**)` — возвращает список всех форм на странице.
    
- `.execute_script(**"****window.scrollTo(****x-coord**, **y-coord****);"**)` — прокручивает документ до указанных координат:
    
    - `**x-coord**` — позиция по горизонтальной оси, которая будет отображена вверху
    - `**y-coord**` — позиция по вертикальной оси, которая будет отображена вверху слева.
- `.execute_script(**"return document.getElementsByClassName(****'container'****);"**)` — возвращает список всех элементов с классом `**'container'**`.
    
- `.execute_script(**"return document.getElementsByTagName(****'container'****);"**)` — возвращает список всех элементов с тегом `**'container'**`.  
     
- `.execute_script(**"return document.getElementById(****'some-id'****);"**)` —  возвращает элемент с указанным **ID** `**'some-id'**`.

# Прокрутка содержимого страницы с помощью класса `Keys`

В Selenium представлены различные действия, которые можно выполнить с помощью клавиатуры. В основном, можно выполнять два действия:

- Нажать клавишу,
- Отпустить нажатую клавишу.

**Нажатие клавиши (Key down)**

```python
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains

driver = ... # инициализация драйвера
ActionChains(driver).key_down(Keys.SHIFT).send_keys("abc").perform()
```

**Отпускание клавиши (Key up)**

```python
ActionChains(driver).key_down(Keys.SHIFT).send_keys("a").key_up(Keys.SHIFT).send_keys("b").perform()
```

Импортируем:  

```
from selenium.webdriver import Keys 

или 

from selenium.webdriver.common.keys import Keys
```

Откроем наш [сайт](http://parsinger.ru/scroll/1/). На нём находится 100 тегов `<input>`, с которыми мы будем взаимодействовать с помощью класса `Keys`. Таким образом взаимодействовать можно только с интерактивными элементами:

- **Интерактивные элементы** предназначены для взаимодействия с пользователем. Это могут быть кнопки, которые можно нажать, ссылки, по которым можно перейти, или поля ввода, в которые можно ввести текст. Они реагируют на действия пользователя, такие как клики мышью или нажатия клавиш. Примеры таких элементов включают в себя кнопки (`<button>`), ссылки (`<a>`), поля ввода (`<input>`) и другие подобные элементы.  
     
- **Неинтерактивные элементы**, напротив, предназначены в основном для отображения информации. Они не реагируют на действия пользователя так, как это делают интерактивные элементы. Примеры включают в себя абзацы с текстом (`<p>`), элементы списка (`<li>`), табличные элементы (`<tr>`, `<td>`) и так далее.

```python
import time
from selenium.webdriver import Keys
from selenium import webdriver
from selenium.webdriver.common.by import By

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    browser.find_element(By.TAG_NAME, 'input').send_keys(Keys.TAB)
    time.sleep(10)
```

`.send_keys(Keys.TAB)` симулирует ввод с клавиатуры. В данном случае симулируется нажатие клавиши TAB. Это может переместить фокус на следующий интерактивный элемент на странице после найденного `<input>`.

Чтобы взаимодействовать подобным образом с остальными элементами `<input>`, нам потребуется цикл `while`, если мы не знаем точного количества элементов, или цикл `for`, если точное количество элементов нам известно(не забываем фактор бесконечной загрузки элементов в боевых условиях).

```python
import time
from selenium.webdriver import Keys
from selenium import webdriver
from selenium.webdriver.common.by import By

with webdriver.Chrome() as browser:
    browser.get('http://parsinger.ru/scroll/1/')
    tags_input = browser.find_elements(By.TAG_NAME, 'input')

    for input in tags_input:
        input.send_keys(Keys.DOWN)
        time.sleep(1)
```

В цикле `for` код проходит по каждому найденному элементу `input` и выполняет следующие действия:

- `input.send_keys(Keys.DOWN)`: "нажимает" клавишу "Вниз" (`DOWN`) в текущем элементе `input`. Это может привести к изменению значения элемента или к другому действию, в зависимости от типа и функционала элемента.

Запустите этот код у себя, и вы увидите, что он поочередно выделяет (берет в фокус) все элементы `<input>` на странице. Страница сайта-тренажера сразу отображает весь список тегов `<input>`.

Для понимания следующего примера откройте любой степ на Степике с более чем 100 комментариями и попробуйте пролистать до самого последнего комментария. Вы увидите несколько загрузок с сервера. Приведенный выше пример с циклом `for` обработал бы только первые 17 элементов, так как они были загружены при открытии страницы. Чтобы решить эту проблему и обрабатывать все подгружаемые элементы, давайте модифицируем этот код.

```python
import time
from selenium.webdriver import Keys
from selenium import webdriver
from selenium.webdriver.common.by import By

with webdriver.Chrome() as browser:
    browser.get(r"https://parsinger.ru/selenium/5.7/3/test/index.html")

    list_input = []      # Инициализируем пустой список для хранения обработанных элементов ввода
    while True:          # Начинаем бесконечный цикл

        # Ищем все элементы input на веб-странице и добавляем их в список input_tags
        input_tags = browser.find_elements(By.TAG_NAME, 'input')

        # Обходим каждый элемент input в списке
        for tag_input in input_tags:
            # Проверяем, не обрабатывали ли мы уже этот элемент ранее
            if tag_input not in list_input:
                tag_input.send_keys(Keys.DOWN)     # Отправляем клавишу "Вниз"
                browser.execute_script("return arguments[0].scrollIntoView(true);", tag_input)
                tag_input.click()                  # Кликаем на элемент
                list_input.append(tag_input)       # Добавляем элемент в список обработанных элементов
```

Обратите внимание, что [страница](https://parsinger.ru/selenium/5.7/3/test/index.html) имитирует "бесконечную" ленту. Перейдите на неё вручную и прокрутите вниз, наблюдая за появляющимися HTML-тегами. В тот момент, когда вы достигнете финала прокрутки, именно так ведёт себя "бесконечная" прокрутка ([Infinite scroll](https://en.wikipedia.org/wiki/Scrolling)).

Пояснение к коду.

- Создаём пустой список `list_input`, который будет использоваться для хранения элементов `<input>`, с которыми уже произведены какие-либо действия (нажатия клавиш или клики).
    
    ```
    list_input = []  # Инициализируем пустой список для хранения обработанных элементов ввода
    ```
    
- Выполняем поиск всех элементов с тегом `<input>` на текущей веб-странице и сохраняет их в список `input_tags`.
    
    ```python
    input_tags = browser.find_elements(By.TAG_NAME, 'input')
    ```
    
- Перебираем каждый найденный элемент `<input>`.
    
    ```python
    for tag_input in input_tags:
    ```
    
- Проверяем, находится ли текущий элемент `tag_input` в списке `list_input`. Если элемент уже был обработан ранее, он будет в этом списке, и следующие действия с ним производиться не будут.
    
    ```python
    if tag_input not in list_input:
    ```
    
- Симулируем нажатие клавиши "Вниз" (`Keys.DOWN`) в текущем элементе `tag_input`, для прокрутки элементов вниз.
    
    ```python
    tag_input.send_keys(Keys.DOWN)  # Отправляем клавишу "Вниз"
    ```
    
- Затем симулируется клик по текущему элементу `tag_input`, если это необходимо вашей задачей.
    
    ```python
    tag_input.click()  # Кликаем на элемент
    ```
    
- Наконец, текущий элемент `tag_input` добавляется в список `list_input`, чтобы в будущем не обрабатывать его повторно.
    
    ```python
    list_input.append(tag_input)  # Добавляем элемент в список обработанных элементов
    ```
    

Запустите последний пример у себя в терминале и понаблюдайте за происходящим. Всё гораздо проще, чем кажется.

#### Доступные к применению клавиши

|   |   |   |
|---|---|---|
|ADD|ALT|ARROW_DOWN|
|ARROW_LEFT|ARROW_RIGHT|ARROW_UP|
|BACKSPACE|BACK_SPACE|CANCEL|
|CLEAR|COMMAND|CONTROL|
|DECIMAL|DELETE|DIVIDE|
|DOWN|UP|ENTER|
|EQUALS|ESCAPE|F1|
|F10|F11|F12|
|F2|F3|F4|
|F5|F6|F7|
|F8|F9|HELP|
|HOME|INSERT|LEFT|
|LEFT_ALT|LEFT_CONTROL|LEFT_SHIFT|
|META|MULTIPLY|NULL|
|NUMPAD0|NUMPAD1|NUMPAD2|
|NUMPAD3|NUMPAD4|NUMPAD5|
|NUMPAD6|NUMPAD7|NUMPAD8|
|NUMPAD9|PAGE_DOWN|PAGE_UP|
|PAUSE|RETURN|RIGHT|
|SEMICOLON|SEPARATOR|SHIFT|
|SPACE|SUBTRACT|TAB|
|END|||


# Прокрутка страницы - **`scroll_by_amount()`**

В версии **Selenium 4.2** появился замечательный метод **.scroll_by_amount()**, который позволяет скролить любое окно на заданное количество пикселей. Этот метод намного упрощает взаимодействие с окнами, в которых присутствует элемент скроллинга. Чтобы этот метод заработал, обновите ваш selenium до последней версии.

- **scroll_by_amount(delta_x, delta_y) -**  
    - - **delta_x**: расстояние по оси X для прокрутки с помощью колеса. Отрицательное значение прокручивается влево.   
    - - **delta_y**: расстояние по оси Y для прокрутки с помощью колеса. Отрицательное значение прокручивается вверх.

Этот метод работает в цепочке событий **ActionChains.**

```
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import By


with webdriver.Chrome() as browser:
    browser.get('https://parsinger.ru/infiniti_scroll_2/')
    div = browser.find_element(By.XPATH, '//*[@id="scroll-container"]/div')
    while True:
        ActionChains(browser).move_to_element(div).scroll_by_amount(1, 500).perform()
```

Как это работает: 

1. В переменной `div` мы определяем окно, которое мы собираемся прокручивать, оно должно иметь полосу прокрутки, иначе ничего не произойдет.
    
2. Цикл `while` для постоянной прокрутки, без цикла скроллинг происходит один раз, что не подойдет для бесконечно подгружаемых элементов.
    
3. `ActionChains(browser)` - создаем цепочку событий.
    
4. `.move_to_element(div)` - перемещаемся к элементу веб драйвера, который мы записали в переменную `div`.
    
5. `.scroll_by_amount(1, 500)`  - скроллинг применяется к элементу в методе  `.move_to_element(div)`.
    

Как итог, мы получаем код, который бесконечно скролит элемент, и нужно думать над его прерыванием. Если мы знаем, какой длины список, мы можем использовать цикл `for`. Если вы уверены, что вам хватит прокрутить элемент 10 раз по 500px, то можно использовать такой подход.

```
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import By

with webdriver.Chrome() as browser:
    browser.get('https://parsinger.ru/infiniti_scroll_2/')
    div = browser.find_element(By.XPATH, '//*[@id="scroll-container"]/div')
    for x in range(10):
        ActionChains(browser).move_to_element(div).scroll_by_amount(1, 500).perform()
```