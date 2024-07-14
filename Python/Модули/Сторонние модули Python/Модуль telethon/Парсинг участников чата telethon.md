## Парсим участников группы

Для работы с этим модулем, была создана отдельная группа в [Telegram](https://t.me/Parsinger_Telethon_Test), просьба **не подписываться**, а только взаимодействовать с ней в рамках курса. Некоторые задачи завязаны на сообщения в группе и на пользователей в ней. Для того, чтобы не сломать задачи себе и другим учащимся **не подписывайтесь** на нее. Telethon позволяет собирать информацию с групп, даже если вы не подписаны на них.

Первый основной метод для получения участников группы - `.get_participants("url_telegram_chat")`. Метод принимает аргумент - ссылку на группу в **telegram,** у которой необходимо получить её участников.

Выполните код у себя в терминале чтобы получить всех участников нашей учебной группы: 

```python
from telethon import TelegramClient, events, sync, connection

r_api = 1*****8 #данные скрыты в целях безопасности
r_hash = 'b******4****0b0****9****8**b' #данные скрыты в целях безопасности

client = TelegramClient('session_name2', r_api, r_hash, system_version="4.10.5 beta x64")
client.start()

participants = client.get_participants('t.me/Parsinger_Telethon_Test')

>>> [User(id=332703068, is_self=True, contact=True, mutual_contact=True, deleted=False, bot=False, bot_chat_history=False, bot_nochats=False, verified=False, restricted=False, min=False, bot_inline_geo=False, support=False, scam=False, apply_min_photo=False, fake=False, access_hash=-5964566557575375003, first_name='Павел', last_name='Хошев', username='Pashikk', phone='79384757505', photo=UserProfilePhoto(photo_id=1428948796795103153, dc_id=2, has_video=False, stripped_thumb=b'\x01\x08\x08I\xb5\x12\xe4\xed\x90\x8c\xf1\x81\xe9E\x14Qa3'), status=UserStatusOffline(was_online=datetime.datetime(2022, 6, 11, 12, 57, 19, tzinfo=datetime.timezone.utc)),
 ........
```

Ссылку на группу можно указать тремя способами. **Важно**: данные можно получить только с той группы, где открыт список пользователей.

1. https://t.me/Parsinger_Telethon_Test -  при указании группы подобным образом, подписка на группу не обязательна;
2. t.me/Parsinger_Telethon_Test - при указании группы подобным образом, подписка на группу не обязательна;
3. Parsinger_Telethon_Test - при указании группы подобным образом учётная запись, с которой вы работаете, должна быть подписана на эту группу.

В ответ мы получаем данные по каждому пользователю. Видим что все поля нам знакомы, мы о них говорили в прошлом степе. Чтобы получить доступ к конкретному полю, мы можем пройтись по возвращенному объекту в цикле и получить доступ к его полям, например к `first_name` и `last_name.`

```python
from telethon import TelegramClient, events, sync, connection

r_api = 1*****8 #данные скрыты в целях безопасности
r_hash = 'b******4****0b0****9****8**b' #данные скрыты в целях безопасности

client = TelegramClient('session_name2', r_api, r_hash, system_version="4.10.5 beta x64")
client.start()

participants = client.get_participants('t.me/Parsinger_Telethon_Test')

for item in participants:
    print(item.first_name, item.last_name)

>>>Павел Хошев
Daxton Perry
Anthony Alexander
William Price
Roger Parks
...
Jordan Jones
Barbara Long
Edith Shaw
Patricia Baker
```

В ответ мы получаем те поля, которые мы запросили у экземпляра класса `client`.