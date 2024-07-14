Для парсинга сообщений из группы или паблика **Telegram** используется два основных метода библиотеки **Telethon**.  Первый метод - это  `.get_messages()`, который возвращает указанное количество сообщений чата. Второй метод - это - `.iter_messages()`, который совершает итерации по каждому сообщению отдельно. У каждого из этих методов есть свои плюсы и минусы. Рассмотрим эти два метода более подробно.

Метод `client.get_messages()` - возвращает `TotalList`(**<class 'telethon.helpers.TotalList'>**) , это список с дополнительными total свойством, который очень сильно похож на именованный кортеж ([_namedtuple_](https://habr.com/ru/post/330034/)), взаимодействие с ним происходит по тому же принципу, что и с `TotalList`. 

Метод `client.iter_messages()` - итерируется по каждому сообщению чата отдельно, удобен, если мы хотим обрабатывать сообщения на лету.

Выполним код ниже и посмотрим на возвращаемый методом `.get_messages()` объект, и разберем его подробнее.

```python
from telethon import TelegramClient, events, sync, connection

r_api = 1*********8
r_hash = 'b***********************b'

with TelegramClient('my', r_api, r_hash, system_version="4.10.5 beta x64") as client:
    all_message = client.get_messages('https://t.me/Parsinger_Telethon_Test')
    for message in all_message:
        print(message)

>>> Message(id=200, peer_id=PeerChannel(channel_id=1795821154), date=datetime.datetime(2022, 6, 11, 9, 40, 26, tzinfo=datetime.timezone.utc), message='56405643545063445516345054346351', out=False, mentioned=False, media_unread=False, silent=False, post=False, from_scheduled=False, legacy=False, edit_hide=False, pinned=False, from_id=PeerUser(user_id=5125814085), fwd_from=None, via_bot_id=None, reply_to=None, media=None, reply_markup=None, entities=[], views=None, forwards=None, replies=MessageReplies(replies=0, replies_pts=213, comments=False, recent_repliers=[], channel_id=None, max_id=None, read_max_id=None), edit_date=None, post_author=None, grouped_id=None, restriction_reason=[], ttl_period=None)
```

- **Message**(
    - **id**=200, - идентификатор этого сообщения;
    - **peer_id**=
        - **PeerChannel**(
            - **channel_id**=1795821154), - **ID** пользователя, группы, канала,  куда было отправлено сообщение;)
    - **date**=
        - datetime.datetime(2022, 6, 11, 9, 40, 26, tzinfo=datetime.timezone.utc), - объект даты, указывает на то, когда сообщение было отправлено;
    - **message**='Текст сообщения', - строковый объект сообщения;
    - **out**=False, - возвращает **True**, если сообщение является исходящим, т.е., отправлено с вашей учетной записи;
    - **mentioned**=False, - возвращает **True**, если, вы были упомянуты в этом сообщении;
    - **media_unread**=False,  - возвращает **True**, если медиа в сообщении были прочитаны;
    - **silent**=False, - возвращает **True**,если сообщение должно уведомить участников звуковым сигналом;
    - **post**=False,  - возвращает **True**, если сообщение является постом в широковещательном канале;
    - **from_scheduled**=False,  - возвращает **True**, если сообщение было создано из заранее запланированного сообщения;
    - **legacy**=False,  - возвращает **True**, если сообщение является устаревшим;
    - **edit_hide**=False,  - возвращает **True**, если метка о редактировании сообщения была скрыта;
    - **pinned**=False,   - возвращает **True**, если сообщение закреплено в настоящее время;
    - **from_id**=
        - **PeerUser**(
            - **user_id**=5125814085),  - **ID** пользователя, группе или канала отправившего сообщение;
    - **fwd_from**=None,  - возвращает **True**, если сообщение является репостом из другого чата\группы\паблика;
    - **via_bot_id**=None,   - **ID** бота который отправлял это сообщение;
    - **reply_to**=None,  - **ID**  сообщения если это сообщение отвечает на другое;
    - **media**=None, - медиафайлы отправленные в сообщении если такие имеются(фото, видео, документы, GIF, стикеры и т.д)
    - **reply_markup**=None, - разметка для этого сообщения, которое отправлено ботом, чаще всего это кнопки;
    - **entities**=[], - список элементов в этом сообщении, таких как жирный шрифт, курсив, код, гиперссылки и т.д.; 
    - **views**=None, - количество просмотров этого сообщения;
    - **forwards**=None, - сколько раз это сообщение было переадресовано;
    - **replies**= - сколько раз другое сообщение ответило на это сообщение;
        - **MessageReplies**(
            - **replies**=0,
            - **replies_pts**=213,
            - **comments**=False,
            - **recent_repliers**=[],
            - **channel_id**=None,
            - **max_id**=None,
            - **read_max_id**=None),
    - **edit_date**=None, - дата последнего редактирования сообщения;
    - **post_author**=None, - отображаемое имя отправителя сообщения;
    - **grouped_id**=None, - проверяет, принадлежит ли это сообщение к группу сообщений, фото- или видео альбому;
    - **restriction_reason**=[], - список причин, по которым это сообщение было ограничено;
    - **ttl_period**=None - период жизни, настроенный для этого сообщения;)

Запоминать все поля возвращаемого объекта не нужно, для парсинга нам чаще всего необходимы следующие поля с информацией:

- **user_id** - чтобы извлечь **ID** пользователя, отправившего сообщение, необходимо к объекту сообщения применить способ 
    
    `message.from_id.user_id`;
    
- **date** - для того, чтобы извлечь дату отправки сообщения, используется способ `message.date`;
- **message** - чтобы извлечь текст сообщения, применяется два способа,  `message.text` и `message.message`;
- **media** - используется для определения, присутствуют ли медиафайлы в сообщении или нет, как извлекать медиафайлы из сообщения мы поговорим далее;
- Доступ с остальным полям осуществляется подобным образом, с этим проблем у вас возникнуть не должно.

Обратите внимание на один важный момент. В объекте **Message**, хранится **user_id** в цифровом формате, а нам чаще всего необходимо знать его **username**, если он у него имеется. Чтобы конвертировать **user_id** в **username** используется метод `.get_entity(**ID**)`. Функция `.get_entity(**ID**)` возвращает объект **User** со всеми соответствующими этому объекту полями.

Выполнение функции `.get_entity(**ID**)` вернет объект, у которого имеются все необходимые поля, нам осталось извлечь из них **username.**

Код ниже, получает первое сообщение в чате, извлекает из него **user_id** (`message.from_id.user_id`) и передает его в функцию `.get_entity(**ID**)` которая возвращает объект **User**, и уже у этого объекта мы извлекаем username.

```python
from telethon import TelegramClient, events, sync, connection

r_api = 1*****8
r_hash = 'b******b'

with TelegramClient('my', r_api, r_hash) as client:
    all_message = client.get_messages('https://t.me/Parsinger_Telethon_Test')
    for message in all_message:
        id_user = message.from_id.user_id
        print(client.get_entity(id_user).username)

>>> daxton_13246
```

## Функции `.iter_messages()` и `.get_messages()`

Как уже говорилось в прошлом степе, оба эти метода помогают извлекать сообщения из чатов, но делают они это по-разному. В этом степе я бы хотел детальнее обсудить каждый из них, ведь с этими методами вы будете работать часто.

### **`.get_messages()` и** `.iter_messages()`принимают одинаковые аргументы:

- `client.get_messages(**chat**\**group**\**dialog**)` - первый обязательный аргумент, ссылка на телеграмм чат\паблик\диалог;
- `client.get_messages(**limit**)` - количество сообщений, которые необходимо собрать из группы\паблика\диалога, если `limit` не установлен, будет получено первое сообщение в чате;
- `client.get_messages(**offset_date=datetime(2022, 6, 1**))` - принимает объект `datatime` находит все сообщения ранее указанной даты, формат даты(год, месяц, день);
    
- `client.get_messages(**offset_id(id_message**))` - будут извлечены те сообщения, которые предшествуют сообщению с указанным `**id_message**`**;**
    
- `client.get_messages(**max_id=30**)` - будут исключены те сообщения, которые имеют более высокий идентификатор; 
    
- `client.get_messages(**min_id=30**)` - будут исключены  те сообщения, которые имеют более старый идентификатор; 
    
- `client.get_messages(**search='Искомый текст'**)` - строка, которая будет использоваться в качестве поискового запроса;
    
- `client.get_messages(**from_user="@user"**)` -будут возвращены сообщения только этого пользователя. Имя можно указать как **username**, **id_user**, **last_name**, **first_name**;
    
- `client.get_message(**reverse=True**)` - Если установлено значение **True**, сообщения будут возвращаться в обратном порядке (от самого старого к самому новому, а не от нового к самому старому). Это также означает, что значения параметров **offset_id** и **offset_date** меняются местами;
    
- `client.get_messages(**filter**=**InputMessagesFilterPhotos**)` - фильтр используется для возврата сообщения. Например, данный фильтр вернет первое сообщение содержащее фото. Для работы с подобными фильтрами необходимо их импортировать `from telethon.tl.types import InputMessagesFilterPhotos`.
- Другие используемые фильтры (у каждого фильтра в списке, свой одноимённый импорт, по аналогии с `InputMessagesFilterPhotos`):
    - `filter=**InputMessagesFilterChatPhotos**`- вернет первое сообщение, содержащее фото;
    - `filter=**InputMessagesFilterContacts**`- вернет первое сообщение, содержащее контакт пользователя;
    - `filter=**InputMessagesFilterDocument**`- вернет первое сообщение, содержащее документ(pdf, docx, txt т т.д.);
    - `filter=**InputMessagesFilterEmpty**`- вернет первое  сообщение, если его отправитель пуст, скорее всего это работает если пользователь был забанен в группе, а его сообщения не удалены;
    - `filter=**InputMessagesFilterGeo**`- вернет первое сообщение, в котором есть геометка;
    - `filter=**InputMessagesFilterGif**`- вернет первое сообщение, в котором содержится **GIF**;
    - `filter=**InputMessagesFilterMusic**`- вернет первое сообщение, если в нём содержится музыкальный файл;
    - `filter=**InputMessagesFilterMyMentions**`- вернет первое сообщение, в котором есть упоминания учетной записи, с которой происходит поиск;
    - `filter=**InputMessagesFilterPhoneCalls**`- вернет информацию об аудио-звонке;
    - `filter=**InputMessagesFilterPhotoVideo**`- вернет информацию об видео-звонке;
    - `filter=**InputMessagesFilterPhotos**`- используется для фильтрации результатов при поиске сообщений, чтобы получить только сообщения, содержащие фотографии;
    - `filter=**InputMessagesFilterPinned**`- вернет первое закреплённое сообщение;
    - `filter=**InputMessagesFilterRoundVideo**`- вернет первое сообщение, в котором содержится  круглое видео;
    - `filter=**InputMessagesFilterRoundVoice**`- вернет первое сообщение, в котором содержится голосовое сообщение;
    - `filter=**InputMessagesFilterUrl**`- вернет первое сообщение, в котором содержится ссылка;
    - `filter=**InputMessagesFilterVideo**`-  вернет первое сообщение, в котором содержится видео;
    - `filter=**InputMessagesFilterVoice**`- вернет первое сообщение, в котором содержится голосовое сообщение.
- Больше методов в [документации](https://docs.telethon.dev/en/stable/modules/client.html#telethon.client.messages.MessageMethods.iter_messages).

Код ниже найдет первое сообщение, содержащее фотографию, и вернет объект этого сообщения.

```python
from telethon import TelegramClient, events, sync, connection
from telethon.tl.types import InputMessagesFilterPhotos

r_api = 1*********8
r_hash = 'b*****************b'

with TelegramClient('my', r_api, r_hash, system_version="4.10.5 beta x64") as client:
    all_message = client.get_messages('https://t.me/Parsinger_Telethon_Test', filter=InputMessagesFilterPhotos)
    print(all_message)

>>> [Message(id=198, peer_id=PeerChannel(channel_id=1795821154), date=datetime.datetime(2022, 6, 10, 17, 32, 24, tzinfo=datetime.timezone.utc), message='', out=False, mentioned=False, media_unread=False, silent=False, post=False, from_scheduled=False, legacy=False, edit_hide=False, pinned=False, from_id=PeerUser(user_id=5423813689), fwd_from=None, via_bot_id=None, reply_to=None, media=MessageMediaPhoto(photo=Photo(id=5845830650752514229, access_hash=802838475124788912, file_reference=b'\x02k\n\x0eb\x00\x00\x00\xc6b\xa9\xc5\x1f\xa5\xe63\xde>\xe9:O\xf26\x11m\xe1C\x10\xa3', date=datetime.datetime(2022, 6, 10, 17, 32, 24, tzinfo=datetime.timezone.utc), sizes=[PhotoStrippedSize(type='i', bytes=b'\x01\'(\xae\xb7\x93\x0e\xbbO\xd4T\xab|\x0f\xdf\x8b\xf2\xa8|\xa3\xe9M\x0b\xb2E-\xd34\xf5\x02\xdf\x9dj\xff\x00{\x8f\xa8\xa0$\x19\xccsm>\xc6\xaa0\xdc\xfe\xbe\xf4\x9eY$\nw\x0b\x1a;g\x0b\x90\xca\xe3\xdcQU%\x84\xc2\xa0\xa3\x9f~h\xa9RM\\vh\xd4\x11\x0ct\xaaz\x82"\xc6\xbe\xa0\xe7\x15k\xedq\x84\'\x04\xed\xc6\x7f\x1a\xa1}0\x9b\x0c\x01\x03\xde\xa9\x81\x14\x12\xacA\xd8\xa6\xec\xe3\x02\xb4\xa1H\xa7\x88H\xa3\x83\xfaV95\xa5d&\x8e\xdcy`\x00\xc7?7j\x01\xa2\xc3[\xa9\x07"\x8aM\xd2n\xf9\xa5\x18\xc7AE\x16\x11\x07\xda8\xc0\x8dEU\xbcb\xc0\x13\x8f\xc2\x8a)\xbd\x81nWa\x8a\xd0\x8b\r\x12\x9c\x9e\x94QJ,rD\xa0\x00(\xa2\x8a\xab\x8a\xc7'), PhotoSize(type='m', w=320, h=308, size=17285), PhotoSizeProgressive(type='x', w=454, h=437, sizes=[4173, 11874, 22670, 23119, 25022])], dc_id=4, has_stickers=False, video_sizes=[]), ttl_seconds=None), reply_markup=None, entities=[], views=None, forwards=None, replies=MessageReplies(replies=0, replies_pts=213, comments=False, recent_repliers=[], channel_id=None, max_id=None, read_max_id=None), edit_date=None, post_author=None, grouped_id=None, restriction_reason=[], ttl_period=None), total=45]
```

Код ниже найдет и скачает столько сообщений с картинками, сколько указано в аргументе `limit=`, запустите у себя в терминале код ниже и посмотрите как он работает.

```python
from telethon import TelegramClient, events, sync, connection
from telethon.tl.types import InputMessagesFilterPhotos

r_api = 1********8
r_hash = 'b********************b'

with TelegramClient('my', r_api, r_hash, system_version="4.10.5 beta x64") as client:
    all_message = client.get_messages('https://t.me/python_parsing', filter=InputMessagesFilterPhotos, limit=100)
    for message in all_message:
        client.download_media(message) #file='img/' чтобы указать путь для скачивания.
```