### Функция dumps()

Для сериализации данных в `json` строку используется функция `dumps()` из модуля `json`. Для того, чтобы сериализовать данные с ее помощью, достаточно передать в нее аргументом любой сериализуемый Python объект. 
ак как `json` — текстовый формат, то сериализация в него — это по сути преобразование данных в строку.

Приведенный ниже код:

```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data = json.dumps(data)            # сериализуем словарь data в json строку

print(type(json_data))
print(json_data)
```

выводит:

```no-highlight
<class 'str'>
{"name": "Russia", "phone_code": 7, "capital": "Moscow", "currency": "RUB"}
```

Обратите внимание на кавычки, независимо от того, что в Python-словаре мы использовали одинарные, в результирующую строку всегда попадают двойные.

### Функция dump()

В отличие от функции `dumps()`, которая преобразует (сериализует) Python объект в `json` строку, функция `dump()` записывает переданный Python объект в файл.

Приведенный ниже код:

```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

with open('countries.json', 'w') as file:
    json.dump(data, file)
```

создает файл `countries.json` и сохраняет в него информацию из словаря `data` в `json` формате.

Если открыть файл `countries.json`, мы увидим, что `json` выведен в одну строку без форматирования:

```json
{"name": "Russia", "phone_code": 7, "capital": "Moscow", "currency": "RUB"}
```

### Необязательные аргументы indent, sort_keys и separators

Функции записи `dumps()` и `dump()` имеют необязательные аргументы `indent`, `sort_keys` и `separators`, которые можно использовать для более удобного чтения человеком.

Аргумент `indent` задает отступ от левого края. По умолчанию имеет значение `None` для более компактного представления без отступов.

Приведенный ниже код:

```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data1 = json.dumps(data, indent=2)
json_data2 = json.dumps(data, indent=10)

print(json_data1)
print(json_data2)
```

выводит:

```no-highlight
{
  "name": "Russia",
  "phone_code": 7,
  "capital": "Moscow",
  "currency": "RUB"
}
{
          "name": "Russia",
          "phone_code": 7,
          "capital": "Moscow",
          "currency": "RUB"
}
```

Если значением `indent` является строка, то она используется в качестве отступа.

Приведенный ниже код:

```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data = json.dumps(data, indent='++++')

print(json_data)
```

выводит:

```no-highlight
{
++++"name": "Russia",
++++"phone_code": 7,
++++"capital": "Moscow",
++++"currency": "RUB"
}
```

  Отступов также не будет, если значение аргумента `indent` равно 00, отрицательному числу или пустой строке.

Аргумент `sort_keys` задает сортировку ключей в результирующем `json`. По умолчанию имеет значение `False` для более быстрого создания `json`. Если установить значение аргумента в `True`, то ключи будут отсортированы в алфавитном порядке, что особенно удобно, когда ключей много.

Приведенный ниже код:

```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data1 = json.dumps(data, indent=3)
json_data2 = json.dumps(data, indent=3, sort_keys=True)

print(json_data1)
print(json_data2)
```

выводит:

```json
{
   "name": "Russia",
   "phone_code": 7,
   "capital": "Moscow",
   "currency": "RUB"
}
{
   "capital": "Moscow",
   "currency": "RUB",
   "name": "Russia",
   "phone_code": 7
}
```

Аргумент `separators` задает кортеж, состоящий из двух элементов `(item_separator, key_separator)`, которые представляют разделители для элементов и ключей.  По умолчанию аргумент имеет значение `(', ', ': ')`.

Приведенный ниже код:

```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data = json.dumps(data, indent=3, separators=(';', ' = '))

print(json_data)
```

выводит:

```json
{
   "name" = "Russia";
   "phone_code" = 7;
   "capital" = "Moscow";
   "currency" = "RUB"
}
```