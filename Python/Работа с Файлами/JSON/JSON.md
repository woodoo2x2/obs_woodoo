[[Формат JSON]]
[[Сериализация JSON]]
[[Десериализация JSON]]
[[Типы данных JSON]]
[[Кириллица JSON]]
[[Модуль json]]

Официальная документация по формату `json` доступна по [ссылке](https://www.json.org/json-ru.html).
Официальная документация по модулю `json` в Python доступна по [ссылке](https://docs.python.org/3.10/library/json.html).
Для удобного форматирования `json` файлов используйте [сайт](https://jsonformatter.curiousconcept.com).

1. чтобы не возникали проблемы с кодировкой, если в файл передаются данные с русскими буквами, как и во всех других случаях работы с файлом, при открытии нужно принудительно устанавливать кодировку (особенно актуально для OS семейства Windows):
    
    ```
    with open('cats_3.json', 'w', encoding='utf8') as cat_file:
        cat_file.write(json.dumps(cats_dict))
    ```
    
2. при создании `json` файла «вручную» нужно помнить, что в нем нельзя использовать одинарные кавычки. При создании программными средствами нужные кавычки ставятся автоматически
3. ключами словаря в `json` не могут быть кортежи и числа. Но ключ-число не вызовет ошибку при сериализации, он будет просто преобразован в строку
4. помните, что при преобразовании данные будут не всегда того же типа, что были в Python
#Python 