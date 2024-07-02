# `soup.select()`

`soup.**select()**` - это метод Beautiful Soup, который позволяет выполнять поиск элементов HTML с помощью CSS селекторов. Он возвращает список тегов, удовлетворяющих заданным условиям.

Вы можете использовать CSS селекторы, такие как тег, класс, идентификатор и другие атрибуты, чтобы найти нужные элементы в документе HTML. Например, вы можете использовать селектор `p.text-class`, чтобы найти все теги `<p>` с классом `text-class`. Или вы можете использовать селектор `p#text-id`, чтобы найти тег `<p>` с идентификатором `text-id`.

Обратите внимание, что `.select()` всегда возвращает список тегов, даже если вы ищете один тег. Чтобы получить один тег, вы можете использовать индекс **[0]** или метод `select_one()`.

**Основные отличия** `.find()` от `.select()`**:**

- **Метод поиска:** `.find()` ищет элементы на основе имени тега, атрибутов или текста, в то время как `.select()` использует CSS-селекторы.
- **Результат:** `.find()` возвращает первый найденный элемент, который соответствует критериям, в то время как `.select()` возвращает список всех элементов, соответствующих CSS-селектору.
- **Гибкость:** `.select()` предоставляет более широкие возможности для сложных запросов, особенно при работе с вложенными или специфическими структурами HTML.

Синтаксис:  
`soup.**select**(**selector**, namespaces=None, limit=None, ****kwargs**)`

Параметры:

- `**selector(str)**` - Это основной параметр, куда передается строка CSS-селектора. Селекторы могут быть различной сложности, включая теги, классы, идентификаторы, вложенность и т.д. Например, `"div.content p"` выберет все `<p>`, которые находятся внутри `<div>` с классом `content`.
    
    - **Пример использования:** `soup.select("div.content p")`
- **`namespaces(dict, optional)`** - Позволяет указать пространства имен для тегов XML. Это полезно, если вы работаете с XML-документами, которые используют пространства имен.
    
    - **Пример использования:** Если у вас есть XML с пространствами имен, вы можете использовать `namespaces={"ns": "http://example.com"}` и затем использовать CSS-селекторы, например, `soup.select("ns|tag")`.
- **`limit=(int, optional)`** - Определяет максимальное количество элементов, которые будут возвращены. Если `limit` не указан, будут возвращены все найденные элементы. Если указан, возвращается не более указанного количества первых найденных элементов.
    
    - **Пример использования:** `soup.select("p", limit=2)` вернет не более двух элементов `<p>`.

Примеры:

- Тег: `select("p")`
- Класс: `select(".class")`
- Идентификатор: `select("#id")`
- Атрибут: `select("[attribute=value]")`
- Несколько селекторов: `select("p.class")`
- Вложенные элементы: `select("div span")`
- Непосредственные дочерние элементы: `select("div > p")`
- Элементы с несколькими классами: `select(".class1.class2")`
- Элементы с определенными атрибутами: `select("[data-custom]")`
- Псевдоклассы CSS: `select("p:first-of-type")` или `select("p:last-of-type")`
- Соседние элементы: `select("h1 + p")`
- Элементы, содержащие определенный текст:
    
    ```
    elements = soup.select("p")
    elements_with_text = [elem for elem in elements if "определенный текст" in elem.text]
    for elem in elements_with_text:
        print(elem.text)
    ```
    

### **Пример 1**

Используем метод `.select()` у объекта `soup`.

В первом случае выбираются все теги `<p>` с классом `text-class`,

```python
result = soup.select('p.text-class')
```

во втором - теги `<p>` с атрибутом `id`,

```python
​​​​​​​result = soup.select('p#text-id')
```

а в третьем - все теги `<p>` внутри тега `body`. В результате текст этих тегов выводится на экран.

```python
result = soup.select('body p')
```

```
from bs4 import BeautifulSoup

html = """
<html>
    <body>
        <h1>Заголовок 1</h1>
        <p class="text-class">Текст 1</p>
        <p class="text-class">Текст 2</p>
        <p id="text-id">Текст 3</p>
    </body>
</html>
"""

soup = BeautifulSoup(html, 'html.parser')

# Найти все теги `p` с классом `text-class`
result = soup.select('p.text-class')
for tag in result:
    print(tag.text)

print('----разделитель----')

# Найти все теги `p` с атрибутом `id`
result = soup.select('p#text-id')
for tag in result:
    print(tag.text)
    
print('----разделитель----')

# Найти все теги `p` внутри тега `body`
result = soup.select('body p')
for tag in result:
    print(tag.text)
```

Вывод:

```
Текст 1
Текст 2
----разделитель----
Текст 3
----разделитель----
Текст 1
Текст 2
Текст 3
```

### **Пример 2**

Выбараем  теги с классом `"highlight"`.

```python
highlighted_paras = soup.select(".highlight")
```

Также выбираем все теги `<p>`, находящиеся внутри элемента с идентификатором `"div1"`.

```python
div_paras = soup.select("#div1 p")
```

```
from bs4 import BeautifulSoup

html = """
<html>
  <body>
    <p class="highlight">This is a highlighted paragraph.</p>
    <p>This is a normal paragraph.</p>
    <div id="div1">
      <p>This is a paragraph in a div.</p>
    </div>
  </body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")

# Выберем все параграфы с классом "highlight"
highlighted_paras = soup.select(".highlight")
for para in highlighted_paras:
    print(para.text)

print('----разделитель----')

# Выберем все параграфы, находящиеся внутри элемента с идентификатором "div1"
div_paras = soup.select("#div1 p")
for para in div_paras:
    print(para.text)
```

Вывод:

```
This is a highlighted paragraph.
----разделитель----
This is a paragraph in a div.
```

### **Пример 3**

Выбираем теги с классом `"highlight"`, которые находятся внутри элементов с идентификатором `"div1"` или `"div2"`.

```
highlighted_paras = soup.select("#div1 .highlight, #div2 .highlight")
```

Так же код печатает текст выбранных параграфов.

```
from bs4 import BeautifulSoup

html = """
<html>
  <body>
    <div id="div1">
      <p class="highlight">This is a highlighted paragraph in div1.</p>
      <p>This is a normal paragraph in div1.</p>
    </div>
    <div id="div2">
      <p class="highlight">This is a highlighted paragraph in div2.</p>
      <p>This is a normal paragraph in div2.</p>
    </div>
  </body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")

# Выберем все параграфы с классом "highlight", которые 
# находятся внутри элементов с идентификатором "div1" или "div2"
highlighted_paras = soup.select("#div1 .highlight, #div2 .highlight")
print("Highlighted paragraphs:")
for para in highlighted_paras:
    print(para.text)
```

Вывод:

```
Highlighted paragraphs:
This is a highlighted paragraph in div1.
This is a highlighted paragraph in div2.
```

### **Пример 4**

Выбираем все теги с классом `"highlight"`, с помощью атрибута.

```
highlighted_paras = soup.select("p[class='highlight']")
```

```
from bs4 import BeautifulSoup

html = """
<html>
  <body>
    <div id="div1">
      <p class="highlight">This is a highlighted paragraph in div1.</p>
      <p>This is a normal paragraph in div1.</p>
    </div>
    <div id="div2">
      <p class="highlight">This is a highlighted paragraph in div2.</p>
      <p>This is a normal paragraph in div2.</p>
    </div>
  </body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")

# Выберем все параграфы с классом "highlight"
highlighted_paras = soup.select("p[class='highlight']")
print("Highlighted paragraphs:")
for para in highlighted_paras:
    print(para.text)
```

 Вывод:

```
Highlighted paragraphs:
This is a highlighted paragraph in div1.
This is a highlighted paragraph in div2.
```

#ПарсингPython 