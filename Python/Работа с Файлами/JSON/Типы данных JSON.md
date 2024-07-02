## Типы данных в json

Приведенный ниже код:

```python
import json

json_data = '''
{
   "name": "Russia",
   "phone_code": 7,
   "latitude": 60.0,
   "capital": "Moscow",
   "timezones": ["Anadyr", "Barnaul", "Moscow", "Kirov"],
   "translations": {
      "nl": "Rusland",
      "hr": "Rusija",
      "de": "Russland",
      "es": "Rusia",
      "fr": "Russie",
      "it": "Russia"
   }
}'''

data = json.loads(json_data)

print(type(data['name']))
print(type(data['phone_code']))
print(type(data['latitude']))
print(type(data['timezones']))
print(type(data['translations']))
```

выводит:

```no-highlight
<class 'str'>
<class 'int'>
<class 'float'>
<class 'list'>
<class 'dict'>
```

Таким образом модуль `json` автоматически определяет тип значения при десериализации. Такая автоматическая работа с типами данных выгодно отличает `json` от `csv`, при работе с которым таких автоматических преобразований нет.

### Изменение типа данных

Еще один важный аспект преобразования данных в формат JSON: данные не всегда будут того же типа, что исходные данные в Python. Например, кортежи при записи в JSON превращаются в списки.

Приведенный ниже код:

```python
import json

data = {
        'name': 'Russia', 
        'phone_code': 7,
        'latitude': 60.0,
        'capital': 'Moscow',
        'timezones': ('Anadyr', 'Barnaul', 'Moscow', 'Kirov')
       }

json_data = json.dumps(data)        # преобразуем dict в json
new_data = json.loads(json_data)    # преобразуем json в dict

print(data == new_data)
```

выводит:

```python
False
```

Так происходит из-за того, что в JSON используются другие типы данных, и не для всех типов данных Python есть соответствия.

Таблица конвертации типов данных Python в JSON:

|Python|JSON|
|---|---|
|`dict`|`object`|
|`list, tuple`|`array`|
|`str`|`string`|
|`int, float`|`number`|
|`True`|`true`|
|`False`|`false`|
|`None`|`null`|

Таблица конвертации JSON в типы данных Python:

|JSON|Python|
|---|---|
|`object`|`dict`|
|`array`|`list`|
|`string`|`str`|
|`number (int)`|`int`|
|`number (real)`|`float`|
|`true`|`True`|
|`false`|`False`|
|`null`|`None`|

### Ограничение по типам данных

В формат JSON нельзя записать словарь, у которого ключи – кортежи.

Приведенный ниже код:

```python
import json

data = {
        'beegeek': 2018,
        ('Timur', 'Guev'): 29,
        ('Arthur', 'Kharisov'): 20,
        'stepik': 2013
       }

json_data = json.dumps(data)        # преобразуем dict в json

print(json_data)
```

генерирует ошибку:

```no-highlight
TypeError: keys must be str, int, float, bool or None, not tuple
```

С помощью необязательного аргумента `skipkeys` можно игнорировать подобные ключи.

Приведенный ниже код:

```python
import json

data = {
        'beegeek': 2018,
        ('Timur', 'Guev'): 29,
        ('Arthur', 'Kharisov'): 20,
        'stepik': 2013
       }

json_data = json.dumps(data, skipkeys=True)        # преобразуем dict в json

print(json_data)
```

выводит:

```json
{"beegeek": 2018, "stepik": 2013}
```

Кроме того, в JSON ключами словаря могут быть только строки. Но, если в словаре Python использовались числа, булевы значения или None, то ошибки не будет, вместо этого они будут преобразованы в строки.

Приведенный ниже код:

```python
import json

data = {1: 'Timur', False: 'Arthur', None: 'Ruslan'}
json_data = json.dumps(data)

print(json_data)
```

выводит:

```json
{"1": "Timur", "false": "Arthur", "null": "Ruslan"}
```