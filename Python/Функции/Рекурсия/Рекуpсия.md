
Ранее мы встречались с функциями, которые вызывают другие функции. Рассмотрим функцию `get_stat()`, которая принимает в качестве аргумента непустой список целых чисел и возвращает кортеж, состоящий из трех значений: минимальный элемент списка, максимальный элемент списка и среднее арифметическое значение всех элементов списка.

Приведенный ниже код:

```python
def get_stat(numbers):
    minimum = min(numbers)
    maximum = max(numbers)
    average = sum(numbers)/len(numbers)

    return (minimum, maximum, average)

print(get_stat([1, 2, 3, 4, 5]))
print(get_stat([7]))
```

выводит:

```no-highlight
(1, 5, 3.0)
(7, 7, 7.0)
```

Как мы видим, написанная нами функция `get_stat()` вызывает функции: [[Функция min()]], [[Функция max()]], [[Функция sum()]], [[Функция len()]]. На практике такая ситуация очень распространена.

Бывают случаи, когда функция также вызывает саму себя. Такая функция называется рекурсивной.

   Рекурсивная функция – это функция, которая вызывает саму себя.

Рассмотрим определение рекурсивной функции `message()`, которая вызывает саму себя.

Приведенный ниже код:

```python
def message():
    print('Это рекурсивная функция')
    message()

message()
```

выводит:

```no-highlight
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
...
```

И этот результат будет повторяться бесконечно (почти).

Функция `message()` выводит на экран строку текста `Это рекурсивная функция`, а затем вызывает саму себя. При каждом вызове функцией самой себя цикл повторяется. Несложно заметить, что при такой реализации функции `message()` в ней не предусмотрен способ остановки рекурсивных вызовов. Эта функция выглядит как бесконечный цикл, поскольку отсутствует программный код, который остановил бы ее бесконечные вызовы.

В ситуации когда не предусмотрен способ остановки рекурсивных вызовов происходит переполнение [[Стек|стека]] и возбуждается исключение `RecursionError`.

Подобно циклу, рекурсивная функция должна иметь определенный способ управлять количеством своих повторов.

Приведенный ниже код:

```python
def message(times):
    if times > 0:
        print('Это рекурсивная функция')
        message(times - 1)

message(5)
```

выводит:

```no-highlight
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
Это рекурсивная функция
```

Теперь функция `message()` принимает аргумент `times`, который задает количество раз, которые функция должна выводить сообщение. Строка текста `Это рекурсивная функция` будет выводиться до тех пор, пока `times` больше нуля, при этом функция будет вызывать саму себя повторно, передавая уменьшенный на единицу аргумент.

Во время каждого вызова функции `message()` в оперативной памяти создается новый экземпляр переменной `times`. При первом вызове функции `times` имеет значение 55. Когда функция себя вызывает, создается новый экземпляр переменной `times` и в него передается значение 44. Этот цикл повторяется до тех пор, пока в функцию в качестве аргумента не будет передан 00.

![](https://ucarecdn.com/78df7caa-efc9-46d1-8368-bfaf44ff7c71/)

Как видно из рисунка, функция `message()` вызывается шесть раз. В первый раз она вызывается из основной программы, а остальные пять раз она вызывает саму себя. Количество раз, которые функция вызывает саму себя, называется **глубиной рекурсии**. В этом примере глубина рекурсии равняется пяти. Когда функция достигает своего шестого вызова, значение переменной `times` равно 00. В этой точке условное выражение оператора `if` становится ложным, и поэтому функция завершает свою работу. Поток управления программы возвращается из шестого экземпляра функции в точку в пятом экземпляре непосредственно после вызова рекурсивной функции.

Поскольку после вызова рекурсивной функции больше нет инструкций, пятый экземпляр функции `message()` возвращает поток управления программы назад в четвертый экземпляр функции и т.д.

Давайте модифицируем нашу функцию `message()`, добавив строку кода, которая выводит текущее значение переменной `times` после рекурсивного вызова.

Приведенный ниже код:

```python
def message(times):
    if times > 0:
        print('Это рекурсивная функция.')
        message(times - 1)
        print(times)

message(5)
```

выводит:

```no-highlight
Это рекурсивная функция.
Это рекурсивная функция.
Это рекурсивная функция.
Это рекурсивная функция.
Это рекурсивная функция.
1
2
3
4
5
```

Обратите внимание на то, что сначала печатается значение 1, затем 2 и так далее до 5.





 Рекурсия может оказаться мощным инструментом для решения повторяющихся задач. Задача может быть решена на основе рекурсии, если ее разделить на уменьшенные задачи, которые по структуре идентичны общей задаче.

* Обратите внимание, что рекурсия никогда не является непременным условием для решения задачи. Любая задача, которая может быть решена рекурсивно, также может быть решена на основе цикла, и наоборот.

 Рекурсивные алгоритмы обычно менее эффективны, чем итеративные алгоритмы. Это связано с тем, что процесс вызова функции требует выполнения компьютером нескольких действий. Эти действия включают выделение памяти под параметры и локальные переменные и для хранения адреса местоположения программы. Такие действия, которые иногда называются накладными расходами, происходят при каждом вызове функции. Накладные расходы не требуются при использовании цикла.

* Некоторые повторяющиеся задачи легче решаются на основе рекурсии, чем на основе цикла. Там, где цикл приводит к более быстрому времени исполнения, программист может быстрее разработать рекурсивный алгоритм. В целом рекурсивная функция работает следующим образом:

- если в настоящий момент задача может быть решена без рекурсии, то функция ее решает
- если в настоящий момент задача не может быть решена, то функция ее сводит к уменьшенной и при этом аналогичной задаче и вызывает саму себя для решения этой уменьшенной задачи

Для того чтобы применить такой подход, во-первых, мы идентифицируем по крайней мере один случай, в котором задача может быть решена без рекурсии. Он называется **базовым случаем**. Во-вторых, мы определяем то, как задача будет решаться рекурсивно во всех остальных случаях. Это называется **рекурсивным случаем**. В рекурсивном случае мы все время должны сводить задачу к уменьшенному варианту исходной задачи. С каждым рекурсивным вызовом задача уменьшается. В результате будет достигнут базовый случай, и рекурсия прекратится.

 

 Аппаратный стек используется для нужд выполняющейся программы: хранения переменных и вызова функций. При вызове функции процессор помещает в стек адрес команды, следующей за командой вызова функции, — «адрес возврата» из функции. По команде возврата из функции из стека извлекается адрес возврата и осуществляется переход по этому адресу.