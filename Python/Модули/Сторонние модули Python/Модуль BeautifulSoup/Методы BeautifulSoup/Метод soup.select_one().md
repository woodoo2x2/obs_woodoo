`soup.**select_one()**` - это метод, который позволяет выбрать один элемент на основе CSS-селектора. Он возвращает первый найденный элемент, удовлетворяющий условию, или `None`, если ничего не было найдено. Этот метод полезен, когда вы знаете, что только один элемент должен удовлетворять условию.

`.select_one()` принимает тот же аргумент, что и `.select()`, т.е. CSS-селекторы.

Разница между ними в том, что `.select_one()` возвращает только первый найденный тег, в то время как `.select()` возвращает список всех найденных тегов.

Синтаксис:  
`select_one(**selector**)`

Параметры:  
Метод `.select_one()` принимает один аргумент - CSS-селектор `(**selector**)`в виде строки. Он используется для поиска первого элемента в HTML или XML документе, который соответствует этому селектору.

Примеры:

- Тег: `select_one("p")`
- Класс: `select_one(".class")`
- Идентификатор: `select_one("#id")`
- Атрибут: `select_one("[attribute=value]")`
- Несколько селекторов: `select_one("p.class")`
- Вложенные элементы: `select_one("div span")`
- Непосредственные дочерние элементы: `select_one("div > p")`
- Элементы с несколькими классами: `select_one(".class1.class2")`
- Элементы с определенными атрибутами: `select_one("[data-custom]")`
- Псевдоклассы CSS: `select_one("p:first-of-type")` или `select_one("p:last-of-type")`
- Соседние элементы: `select_one("h1 + p")`

### **Пример 1**

Код использует метод `select_one` для выбора первого тега `<p>` с атрибутом `«class»`, равным `«highlight»`.

```python
highlighted_para = soup.select_one("p[class='highlight']")
```

Затем текст выбранного тега извлекается и печатается.

```python
print(highlighted_para.text)
```

```python
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

# Выберем первый параграф с классом "highlight"
highlighted_para = soup.select_one("p[class='highlight']")
print("Highlighted paragraph:")
print(highlighted_para.text)
```

### **Пример 2** 

Используем метод `select_one` для выбора первого элемента с классом `"container"`.

```
first_container = soup.select_one('.container')
```

```python
from bs4 import BeautifulSoup

html_doc = """
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <div class="container">
            <p>This is a paragraph.</p>
            <p>This is another paragraph.</p>
        </div>
        <div class="container">
            <p>This is a third paragraph.</p>
            <p>This is a fourth paragraph.</p>
        </div>
    </body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')

# Выберем первый элемент с классом "container"
first_container = soup.select_one('.container')
print(first_container)
```

Для приведенного документа это будет первый же <div>:

```
<div class="container">
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>
</div>
```

### **Пример 3**

Выбираем первый элемент с селектором `".container p.highlight"`.

```
highlighted_paragraph = soup.select_one('.container p.highlight')
```

```python
from bs4 import BeautifulSoup

html_doc = """
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <div class="container">
            <p>This is a paragraph.</p>
            <p>This is another paragraph.</p>
        </div>
        <div class="container">
            <p class="highlight">This is a highlighted paragraph.</p>
            <p>This is a fourth paragraph.</p>
        </div>
    </body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')

# Выберем первый элемент p с классом «highlight», 
# расположенный внутри элемента с классом «container».
highlighted_paragraph = soup.select_one('.container p.highlight')
print(highlighted_paragraph)
```

Для приведенного документа это будет первый элемент тега `<p>` с классом `"highlight"` , который был обнаружен внутри второго элемента тега `<div>` с классом `"container"`. 

```
<p class="highlight">This is a highlighted paragraph.</p>
```
#ПарсингPython 