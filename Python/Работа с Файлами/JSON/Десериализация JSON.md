### Функция loads()

Для десериализации данных нужно использовать функцию `loads()`. Ее аргумент — это строка с данными в формате `json`.

Приведенный ниже код:

```python
import json

json_data = '{"name": "Russia", "phone_code": 7, "capital": "Moscow", "currency": "RUB"}'

data = json.loads(json_data)
print(type(data))
print(data)
```

выводит:

```no-highlight
<class 'dict'>
{'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}
```

Как видно из примера, функция `loads()` десериализует `json` строку в словарь.

В случае если строка для десериализации содержит данные с ошибкой, то модуль `json` не сможет правильно прочитать такую строку, и программа завершится с ошибкой.

Приведенный ниже код (запятая стоит после двоеточия):

```python
import json

json_data = '{"name":, "Russia", "phone_code": 7, "capital": "Moscow", "currency": "RUB"}'  # строка

data = json.loads(json_data)
print(type(data))
print(data)
```

приводит к возникновению ошибки (исключения):

```no-highlight
json.decoder.JSONDecodeError
```

### Функция load()

В отличие от функции `loads()`, которая в качестве аргумента принимает строку с данными в формате `json`, функция `load()` принимает файловый объект и возвращает его десериализованное содержимое.

Пусть файл `data.json` имеет следующее содержимое:

```json
{
  "name": "Russia",
  "phone_code": 7,
  "capital": "Moscow",
  "cities": ["Abakan", "Almetyevsk", "Anadyr", "Anapa", "Arkhangelsk", "Astrakhan"],
  "currency": "RUB"
}
```

Приведенный ниже код:

```python
import json

with open('data.json') as file:
    data = json.load(file)                # передаем файловый объект
    for key, value in data.items():
        if type(value) == list:
            print(f'{key}: {", ".join(value)}')
        else:
            print(f'{key}: {value}')
```

читает содержимое `data.json` файла в словарь `data` и выводит его содержимое:

```no-highlight
name: Russia
phone_code: 7
capital: Moscow
cities: Abakan, Almetyevsk, Anadyr, Anapa, Arkhangelsk, Astrakhan
currency: RUB
```