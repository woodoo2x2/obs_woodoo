## Парсим описания профиля участника группы

Блок Описание, или **Description**, или "**О себе**" имеется у каждого профиля, и я не нашел опции спрятать этот блок, чаще там просто отсутствует информация. Если вам пока не понятно, это тот блок, что в рамке на скриншоте ниже. Вот именно его мы и будем учится парсить в следующих степах.

![](https://ucarecdn.com/c58281a1-5c47-4cbf-8dd8-3e79f534355a/)

В прошлых уроках для получения информации о пользователе мы использовали метод `.get_participants()`, который возвращает **TotalList** со всеми участниками группы. У этого метода есть один серьезный недостаток: он не работает в группах  с числом участников больше 4 000. Поэтому нам необходим другой метод, а именно `.iter_participants(channel)`, который принимает в аргументы ссылку на группу. 

Метод `.iter_participants(channel)`  итерирует по участникам группы, сразу отдавая результат,  поэтому это то, что нам нужно. Также следует отметить, что [метод](https://stepik.org/lesson/734519/step/1?unit=736082) `.get_me()`, который возвращает информацию о пользователе, не имеет поля about, в котором хранится информация "О себе". Чтобы получить доступ к этому полю, нам потребуется импортировать еще один модуль, который работает с пользовательским профилем и имеет больше полей для работы с ними. 

**Импортируем:**

`from telethon.tl.functions.users import GetFullUserRequest`

**Синтаксис использования**:  `client(GetFullUserRequest(user))` , где `user` - это ссылка на профиль пользователя.

Код ниже выполняет итерацию по всем участникам группы **Parsinger_Telethon_Test**, и собирает информацию хранящуюся в поле "О Себе",  с помощью атрибута **about** класса **TelegramClient.**

```python
from telethon import TelegramClient, events, sync, connection
from telethon.tl.functions.users import GetFullUserRequest

r_api = 1*****8 #данные скрыты в целях безопасности
r_hash = 'b******4****0b0****9****8**b' #данные скрыты в целях безопасности

with TelegramClient('my', r_api, r_hash, system_version="4.10.5 beta x64") as client:
    users = client.iter_participants('Parsinger_Telethon_Test')
    for user in users:
        user_full_about = client(GetFullUserRequest(user))
        print(user_full_about.full_user.about)
```

Давайте посмотрим на все поля атрибутов возвращаемого объекта у класса `GetFullUserRequests()`. Обратите внимание, что возвращаемый объект похож на объект из первого [степа](https://stepik.org/lesson/734519/step/1?unit=736082), только там он назывался `User(),` а тут название `UserFull()`, это нам красноречиво намекает на то, что `UserFull()` обладает расширенным количеством полей. Чтоб не повторяться, я выделил лишь те, которые отличаются от объекта `User()`. Для парсинга нам почти всегда нужно всего одно поле, **about**, с остальными вы можете познакомится сомостоятельно.

- UserFull(
    - user=User(
        - id=2********2, 
        - is_self=True, 
        - contact=True, 
        - mutual_contact=True, 
        - deleted=False, 
        - bot=False, 
        - bot_chat_history=False, 
        - bot_nochats=False, 
        - verified=False, 
        - restricted=False, 
        - min=False, 
        - bot_inline_geo=False, 
        - support=False, 
        - scam=False, 
        - apply_min_photo=True, 
        - fake=False, 
        - access_hash=32**************2, 
        - first_name='Павел', 
        - last_name=Хошев, 
        - username='Pashikk', 
        - phone=+7 *** 5** 5* 0* ,
    - photo=UserProfilePhoto(
        - photo_id=9*************5, 
        - dc_id=2,
        - has_video=False, 
        - stripped_thumb=b'\****\x08\x0b\xedA!9\xcf~\xd4QE+\xb1(\xa7\xd0'), 
        - **status**=UserStatusRecently(), 
        - bot_info_version=None, 
        - restriction_reason=[], 
        - bot_inline_placeholder=None, 
        - lang_code=None), 
    - settings=**PeerSettings**(
        - **report_spam**=False, 
        - **add_contact**=False, 
        - **block_contact**=False, 
        - **share_contact**=False, 
        - **need_contacts_exception**=True, 
        - **report_geo**=False, 
        - **autoarchived**=False, 
        - **invite_members**=False, 
        - **geo_distance**=None), 
    - notify_settings=**PeerNotifySettings**(
        - **show_previews**=None, 
        - **silent**=None, 
        - **mute_until**=None, 
        - **sound**=None), 
    - **common_chats_count**=3, 
    - **blocked**=False, 
    - **phone_calls_available**=True, 
    - **phone_calls_private**=False, 
    - **can_pin_message**=True, 
    - **has_scheduled**=False, 
    - **video_calls_available**=True, 
    - **about**='Описание профиля', 
    - profile_photo=**Photo**(
        - **id**=9**************5, 
        - **access_hash**=5***********48,
        - **file_reference**=b'\x00b\xa4\xea\xf3w&ji\xd0|\xf4\x10\xdc+\xfe\\<`\x1d\xbe',