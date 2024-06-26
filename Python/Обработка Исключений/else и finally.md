## Необязательный блок else

Помимо блоков `try` и `except`, в инструкции `try-except` может также использоваться необязательный блок `else`.

Блок `else` размещается после последнего `ехсерt` блока и содержит программный код, который **выполняется только в том случае**, если при выполнении кода в `trу` блоке ошибок (исключений) не было.

Шаблон инструкции `try-except` в этом случае такой:

```python
try:
    # контролируемый код
except тип_ошибки_1:
    # код обработки ошибки (исключения)
except тип_ошибки_2:
    # код обработки ошибки (исключения)
...
except тип_ошибки_n:
    # код обработки ошибки (исключения)
else:
    # код для случая, если ошибки (исключения) не было
```

Рассмотрим программный код:

```python
try:
    num = int(input())
    print('Квадрат числа равен:', num ** 2)
except ValueError:
    print('Вы ввели некорректные данные!')
else:
    print('Ошибки не произошло!')

print('Работа программы завершена!')
```

Результат выполнения этого программного кода зависит от того, какое значение будет введено. Рассмотрим две ситуации.

**Ситуация 1.** На вход подается целое число, например `10`. Тогда результатом выполнения кода будет:

```no-highlight
Квадрат числа равен: 100
Ошибки не произошло!
Работа программы завершена!
```

**Ситуация 2.** На вход подается не число, а строка, например, `abc`. Тогда результатом выполнения кода будет:

```no-highlight
Вы ввели некорректные данные!
Работа программы завершена!
```

![](https://ucarecdn.com/fbddb451-8c79-4b89-abea-c758a0c9be10/)Блок `else` в конструкции `try-except` подобен блоку `else` в конструкциях `for/while`. Он срабатывает если в контролируемом коде не произошло ошибок (если тело цикла завершилось штатным способом, без `break`).

## Необязательный блок finally

Помимо необязательного блока `else`, в инструкции `try-except` можно также использовать необязательный блок `finally`.

Блок `finally` размещается после последнего `ехсерt` блока, либо после блока `else`, если он присутствует, и содержит программный код, который **выполняется в любом случае**, независимо от того, возникла ошибка (исключение) при выполнении кода `trу` блока или нет.

Шаблон инструкции `try-except` в этом случае такой:

```python
try:
    # контролируемый код
except тип_ошибки_1:
    # код обработки ошибки (исключения)
except тип_ошибки_2:
    # код обработки ошибки (исключения)
...
except тип_ошибки_n:
    # код обработки ошибки (исключения)
finally:
    # код, который выполняется всегда
```

Рассмотрим программный код:

```python
try:
    num = int(input())
    print('Квадрат числа равен:', num ** 2)
except ValueError:
    print('Вы ввели некорректные данные!')
finally:
    print('Блок кода выполняется всегда!')

print('Работа программы завершена!')
```

Результат выполнения этого программного кода зависит от того, какое значение будет введено. Рассмотрим две ситуации.

**Ситуация 1.** На вход подается целое число, например, `10`. Тогда результатом выполнения кода будет:

```no-highlight
Квадрат числа равен: 100
Блок кода выполняется всегда!
Работа программы завершена!
```

**Ситуация 2.** На вход подается не число, а строка, например, `abc`. Тогда результатом выполнения кода будет:

```no-highlight
Вы ввели некорректные данные!
Блок кода выполняется всегда!
Работа программы завершена!
```

![](https://ucarecdn.com/112faa61-2575-4bb5-90e6-6875b99923fb/)   Блок `finally` располагается после блока `else`, в случае присутствия последнего.

Блок `finally` особенно удобен при работе с файлами, которые нужно обязательно закрывать, независимо от того, произошла ошибка (исключение) или нет.

Приведенный ниже код:

```python
try:
    file = open('data.txt', encoding='utf-8')
    try:
        text = file.read()
    except:
        print('При чтении из файла произошла ошибка!')
    else:
        print('Чтение из файла прошло успешно!')
    finally:
        file.close()
except FileNotFoundError:
    print('Файл с указанным именем не найден!')
```

демонстрирует возможность использования блока `finally`. В этом коде файл будет закрыт в любом случае, вне зависимости от того, произошла ошибка или нет. Обратите также внимание на вложенность блоков `try-except`.

![](https://ucarecdn.com/d405fdd2-7f67-4d33-ad45-75e2b4eb632b/)   Блоки `try-except` можно вкладывать один в другой. Python никак нас не ограничивает в количестве таких вложений.

Блок `finally` может также использоваться без блоков `except` и `else`. В этом случае, если в блоке `try` возникает ошибка (исключение), то сначала выполняется блок `finally`, а затем ошибка (исключение) продолжает «всплывание» к обработчику более высокого уровня.

```python
try:
    file = open('data.txt', encoding='utf-8')
    try:
        text = file.read()
    finally:
        file.close()
except FileNotFoundError:
    print('Файл с указанным именем не найден!')
except:
    print('Произошла ошибка!')
```

## Общий шаблон инструкции try-except

Общий шаблон инструкции `try-except`, включая все блоки, имеет вид:

```python
try:
    # контролируемый код
except тип_ошибки_1:
    # код обработки ошибки (исключения)
except тип_ошибки_2:
    # код обработки ошибки (исключения)
...
except тип_ошибки_n:
    # код обработки ошибки (исключения)
else:
    # код для случая, если ошибки не было
finally:
    # код, который выполняется всегда
```

## Примечания

**Примечание 1.** Необходимо заметить, что при наличии ошибки (исключения) и отсутствии блока `except`, инструкции внутри блока `finally` будут выполнены, но ошибка (исключение) не будет обработана. Она продолжит «всплывание» к обработчику более высокого уровня. Если пользовательский обработчик отсутствует, то управление передается обработчику по умолчанию, который прерывает выполнение программы и выводит сообщение об ошибке.

```python
try:
    х = 10 / 0
finally:
    print('Блoк finally')
```

выводит:

```no-highlight
Блoк finally
Traceback (most recent call last):
  File "main.py", line 2, in <module>
    х = 10 / 0
ZeroDivisionError: division by zero
```

**Примечание 2.** Инструкции внутри блока `finally` будут выполнены, даже если блок `try` содержит `break, continue, return`.

**Примечание 3.** Следующая особенность работы блока `finally` связана с обработкой ошибок (исключений) внутри функций. Нужно знать, что блок `finally` выполняется до `return`.

Приведенный ниже код:

```python
def func():
    try:
        return 10
    finally:
        print('Выполняется блок finally!')

print(func())
```

выводит:

```no-highlight
Выполняется блок finally!
10
```

**Примечание 4.** Интересна ситуация, когда возврат из функции (`return`) осуществляется как в блоке `try`, так и в блоке `finally`. Поскольку блок `finally` выполняется до `return`, то результирующее значение будет получено именно из блока `finally`.

Приведенный ниже код:

```python
def func():
    try:
        return 10
    finally:
        return 20

print(func())
```

выводит:

```
20
```

**Примечание 5.** Язык Python поддерживает протокол менеджеров контекста с помощью ключевого слова `with`. Этот протокол гарантирует выполнение завершающих действий (например, закрытие файла) вне зависимости от того, произошла ошибка (исключение) внутри блока кода или нет. 

Приведенный код:

```python
with open('data.txt', 'r', encoding='utf-8') as file:
    text = file.read()
```

примерно равнозначен коду:

```python
try:
    file = open('data.txt', 'r', encoding='utf-8')
    try:
        text = file.read()
    finally:
        file.close()
except:
    pass
```

Предпочтение всегда стоит отдавать менеджерам контекста (ключевое слово `with`), а не блоку `try-finally`.

**Примечание 6.** Рассмотрим следующий код:

```python
def f():
    try:
        x = 10
        return x
    finally:
        x = 20
          
print(f())
```

Как мы знаем, код в блоке `finally` выполняется до оператора `return`, поэтому можно предположить, что после выполнения приведенного выше кода будет выведено значение `20`, хотя на самом деле выведено будет `10`. Дело в том, что оператор `return` сохраняет значение возвращаемой переменной, поэтому дальнейшее присвоение переменной `x` нового значения не повлечет за собой изменения возвращаемого функцией значения.

Проиллюстрировать это можно следующий образом:

![](https://ucarecdn.com/b0cd88e9-9285-4fb1-93b7-88840d5a660a/)

Для большего понимания рассмотрим следующий код:

```
def f():
    try:
        x = [10]
        return x
    finally:
        x.append(20)
          
print(f())
```

Теперь в переменной `x` содержится изменяемый объект — список `[10]`. Оператор `return` сохраняет данный список в качестве возвращаемого значения, но мы по-прежнему можем к нему обратиться и, например, добавить в него значение `20`. Так как оператор `return` по-прежнему ссылается на этот же список, после выполнения приведенного выше кода будет выведено `[10, 20]`.

- ветка на сайте stackoverflow с обсуждением данного вопроса доступна по [ссылке](https://stackoverflow.com/questions/62678765/finally-always-runs-just-before-the-return-in-try-block-then-why-update-in-fina)
- полезная статья об устройстве переменных в Python доступна по [ссылке](https://nedbatchelder.com/text/names.html)

**Примечание 7.** Рассмотрим следующий код:

```python
def f():
    try:
        return 10
    except:
        pass
    else:
        return 20
          
print(f())
```

который выводит:

```no-highlight
10
```

Код в блоке `else`, в отличие от кода в блоке `finally`, не выполняется всегда, поэтому после первого вызова оператора `return` со значением `10` функция завершит свое выполнение и уже не перейдет в блок `else`, несмотря на то что никаких исключений не возникло.