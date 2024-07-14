[Telethon](https://docs.telethon.dev/en/stable/) — самая простая, на мой, взгляд библиотека для мониторинга чатов и написания ботов в **Telegram**. С вероятностью 99,99% вы знаете, что такое **Telegram**, сложно представить современного человека, который о нем не слышал. Если вы читаете этот текст, то вы хотите научиться получать данные с Telegram-чатов. Я не буду спрашивать, для каких целей вам это нужно,  нужно - значит нужно, и моя задача - вас этому научить.

**Telethon** - работает с API telegram **client**,что позволяет нам работать с группами/чатами в Telegram как пользователь. Другие библиотеки чаще всего работают с API telegram **bot** и имеют соответствующий функционал.

В этом модуле вы научитесь получать данные с чатов в **Telegram**, и данные пользователей состоящих в этих чатах.

Вы познакомитесь и сможете работать с такими вещами как:

- Авторизация в Telegram;
- Мониторинг пользователей в чате;
- Работа с сообщениями;
    - Отправка сообщений;
    - Итерирование по сообщениям чата;
    - Отправка сообщений с медиа;
    - Загрузка медиа из чужих сообщений;
    - Работа с пользователями чата, получение инфы с профиля;
    - Получение информации о пользователе;
    - И др...

## Установка и импорт библиотеки

## Установка

`pip install telethon` или `pip3 install telethon`

## Импорт

`from telethon import TelegramClient, events, sync, connection`

После установки и импорта можно запустить свой первый код. Запустите этот код , у себя в терминале введите номер телефона, который вы вводили на первом этапе.

Двухфакторная аутентификация не позволит вам авторизоваться, поэтому, перед первым запуском ее необходимо отключить.

Укажите параметр `system_version="4.10.5 beta x64"` если после запуска кода у вас случился разлогин в аккаунте ТГ.

```python
from telethon import TelegramClient, events, sync, connection


api_id = 12345 # Тут укажите полученый ранее api
api_hash = '0123456789abcdef0123456789abcdef' # Тут укажите полученый ранее hash

client = TelegramClient('session_name', api_id, system_version="4.10.5 beta x64")
client.start()
print(client.get_me())
client.disconnect()


>>> ('User(\n'
 '\tid=**270****,\n'
 '\tis_self=True,\n'
 '\tcontact=True,\n'
 '\tmutual_contact=True,\n'
 '\tdeleted=False,\n'
 '\tbot=False,\n'
 '\tbot_chat_history=False,\n'
 '\tbot_nochats=False,\n'
 '\tverified=False,\n'
 '\trestricted=False,\n'
 '\tmin=False,\n'
 '\tbot_inline_geo=False,\n'
 '\tsupport=False,\n'
 '\tscam=False,\n'
 '\tapply_min_photo=False,\n'
 '\tfake=False,\n'
 '\taccess_hash=-5***6***7******3,\n'
 "\tfirst_name='Павел',\n"
 "\tlast_name='Хошев',\n"
 "\tusername='Pashikk',\n"
 "\tphone='79**4**7**5',\n"
 '\tphoto=UserProfilePhoto(\n'
 '\t\tphoto_id=1428948796795103153,\n'
 '\t\tdc_id=2,\n'
 '\t\thas_video=False,\n'
 '\t\t'
 "stripped_thumb=b'\\x01\\x08\\x08I\\xb5\\x12\\xe4\\xed\\x90\\x8c\\xf1\\x81\\xe9E\\x14Qa3'\n"
 '\t),\n'
 '\tstatus=UserStatusOffline(\n'
 '\t\twas_online=datetime.datetime(2022, 6, 8, 15, 37, 5, '
 'tzinfo=datetime.timezone.utc)\n'
 '\t),\n'
 '\tbot_info_version=None,\n'
 '\trestriction_reason=[\n'
 '\t],\n'
 '\tbot_inline_placeholder=None,\n'
 '\tlang_code=None\n'
 ')')


```

Когда вы введете номер, Telegram пришлет вам код подтверждения. Вставьте его в консоль и выполните этот код снова.  В ответ вы получите кортеж с данными о вашей учетной записи Telegram. Если вы получили эти данные, то поздравляю, у вас все получилось.

Давайте детально рассмотрим, что здесь написано. Переменная **api_id** = 12345 хранит ваш персональный **ID**, который вы создали на предыдущем шаге. Переменная **api_hash** = '0123456789abcdef0123456789abcdef” хранит ваш персональный хэш, который вы также создали на предыдущем шаге. `client = TelegramClient('session_name', api_id, api_hash)` - в переменной **client**  мы создали экземпляр класса **TelegramClient**, к которому мы будем применять все методы библиотеки Telethon, где '**session_name**' - любое имя для сессии. `.client.start()` - непосредственно запуск самой сессии.

**UPD:**

```
telethon.errors.rpcerrorlist.ApiIdInvalidError: The api_id/api_hash combination is invalid (caused by SendCodeRequest)
```

**Если вы столкнулись с данной ошибкой, то решить её можно, отключив в настройках телеграмма двухфакторную авторизацию в настройках конфиденциальности.**

![](https://ucarecdn.com/96b27243-0274-4034-a5f1-d973492916ed/)