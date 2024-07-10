```python
from pyrogram import Client
import asyncio

api_id = 2********2
api_hash = "8******************7"
group_url = "parsinger_pyrogram"
app = Client("my_session", api_id=api_id, api_hash=api_hash)


def main():
    with app:
        chat = app.get_chat(group_url)
        print(type(chat))

main()
```

```
<class 'pyrogram.types.user_and_chats.chat.Chat'>
```

Класс `pyrogram.types.user_and_chats.chat.Chat` представляет собой объект чата и содержит различные атрибуты и методы для работы с этим чатом.

Вот некоторые из атрибутов, которые могут быть доступны в объекте `Chat`:

1. `chat.**id**`: Уникальный идентификатор чата.
2. `chat.**type**`: Тип чата (может быть "**private**", "**group**", "**supergroup**" или "**channel**").
3. `chat.**username**`: Имя пользователя чата, если это личный чат, или `@username` канала/супергруппы.
4. `chat.**first_name**` и `**last_name**`: Имя и фамилия пользователя, если это личный чат.
5. `chat.**title**`: Название группы, супергруппы или канала.
6. `chat.**description**`: Описание группы, супергруппы или канала.
7. `chat.**pinned_message**`: Закрепленное сообщение в чате, если оно есть.
8. `chat.**permissions**`: Права участников чата.
9. `chat.**photo**`: Фотография профиля чата, если она установлена.

Этот JSON представляет собой словарь с информацией о чате в Telegram, полученной с использованием библиотеки Pyrogram. Вот описание каждой строки:

Для лучше читаемости, скопируйте этот JSON куда-нибудь где нет ограничений по длине строки.

```python
{
    "_": "Chat",                            # Тип объекта, в данном случае это чат.
    "id": -1001795821154,                   # Уникальный идентификатор чата. (chat.id)
    "type": "ChatType.SUPERGROUP",          # Тип чата, в данном случае это супергруппа. (chat.type)
    "is_verified": false,                   # Флаг, показывающий, верифицирован ли чат. (chat.is_verified)
    "is_restricted": false,                 # Флаг, показывающий, ограничен ли доступ к чату. (chat.is_restricted)
    "is_creator": false,                    # Флаг, показывающий, является ли пользователь создателем чата. (chat.is_creator)
    "is_scam": false,                       # Флаг, показывающий, является ли чат мошенническим. (chat.is_scam)
    "is_fake": false,                       # Флаг, показывающий, является ли чат фальшивым. (chat.is_fake)
    "title": "Parsinger_Telethon_Test",     # Название чата. (chat.title)
    "username": "Parsinger_Telethon_Test",  # Имя пользователя чата (если это канал или супергруппа). (chat.username)
    "photo": {                              # Фотография профиля чата. (chat.photo)
        "_": "ChatPhoto",                   # Тип объекта, в данном случае это фотография чата. 
        "small_file_id": "AQADAgAD7MMxG4XoQUoAEAIAA57hUMAW____Ft-3784XVesABB4E",  # ID маленькой версии фотографии. (chat.photo.small_file_id)
        "small_photo_unique_id": "AgAD7MMxG4XoQUo",                               # Уникальный ID маленькой версии фотографии. (chat.photo.small_photo_unique_id)
        "big_file_id": "AQADAgAD7MMxG4XoQUoAEAMAA57hUMAW____Ft-3784XVesABB4E",    # ID большой версии фотографии. (chat.photo.big_file_id)
        "big_photo_unique_id": "AgAD7MMxG4XoQUo"                                  # Уникальный ID большой версии фотографии. (chat.photo.big_photo_unique_id)
    },
    "description": "dgf8974503g+3804553g4+556034+5j08456670k4865;740359-'4=0654544066f5450646554k0456678064+g466445+97460h4567321645k565789060k67584890546673574560+7tg456857+45680g766409j328740658974=-60954=564365405426531405g325464562503d4535424365067h74563765j5646857k40063",  # Описание чата. (chat.description)
    "dc_id": 2,                             # ID центра данных, где хранится информация о чате. (chat.dc_id)
    "has_protected_content": false,         # Флаг, показывающий, содержит ли чат защищенный контент. (chat.has_protected_content)
    "pinned_message": {                     # Закрепленное сообщение в чате. (chat.pinned_message)
        "_": "Message",                     # Тип объекта, в данном случае это сообщение. 
        "id": 330,                          # ID сообщения. (chat.pinned_message.id)
        "from_user": {                      # Пользователь, отправивший сообщение. (chat.pinned_message.from_user)
            "_": "User",                    # Тип объекта, в данном случае это пользователь. 
            "id": 5799846345,               # ID пользователя. (chat.pinned_message.from_user.id)
            "is_self": false,               # Флаг, показывающий, является ли пользователь текущим пользователем. (chat.pinned_message.from_user.is_self)
            "is_contact": false,            # Флаг, показывающий, находится ли пользователь в контактах. (chat.pinned_message.from_user.is_contact)
            "is_mutual_contact": false,     # Флаг, показывающий, является ли пользователь взаимным контактом. (chat.pinned_message.from_user.is_mutual_contact)
            "is_deleted": false,            # Флаг, показывающий, удален ли аккаунт пользователя. (chat.pinned_message.from_user.is_deleted)
            "is_bot": false,                # Флаг, показывающий, является ли пользователь ботом. (chat.pinned_message.from_user.is_bot)
            "is_verified": false,           # Флаг, показывающий, верифицирован ли пользователь. (chat.pinned_message.from_user.is_verified)
            "is_restricted": false,         # Флаг, показывающий, ограничен ли доступ пользователя. (chat.pinned_message.from_user.is_restricted)
            "is_scam": false,               # Флаг, показывающий, является ли аккаунт пользователя мошенническим. (chat.pinned_message.from_user.is_scam)
            "is_fake": false,               # Флаг, показывающий, является ли аккаунт пользователя фальшивым. (chat.pinned_message.from_user.is_fake)
            "is_support": false,            # Флаг, показывающий, является ли пользователь сотрудником поддержки Telegram. (chat.pinned_message.from_user.is_support)
            "is_premium": false,            # Флаг, показывающий, является ли пользователь премиум-пользователем. (chat.pinned_message.from_user.is_premium)
            "first_name": "Larry",          # Имя пользователя. (chat.pinned_message.from_user.first_name)
            "last_name": "Summers",         # Фамилия пользователя. (chat.pinned_message.from_user.last_name)
            "status": "UserStatus.OFFLINE",             # Статус пользователя (в данном случае оффлайн). (chat.pinned_message.from_user.status)
            "last_online_date": "2023-01-24 15:39:57",  # Последняя дата онлайн. (chat.pinned_message.from_user.last_online_date)
            "username": "Larry_Summers",                # Имя пользователя в Telegram. (chat.pinned_message.from_user.username)
            "dc_id": 5,                                 # ID центра данных, где хранится информация о пользователе. (chat.pinned_message.from_user.dc_id)
            "photo": {                                  # Фотография профиля пользователя. (chat.pinned_message.from_user.photo)
                "_": "ChatPhoto",                       # Тип объекта, в данном случае это фотография чата. 
                "small_file_id": "AQADBQADAbExGw3rgFYAEAIAA8mhslkBAAPoL1uFHxdshgAEHgQ",  # ID маленькой версии фотографии. (chat.pinned_message.from_user.photo.small_file_id)
                "small_photo_unique_id": "AgADAGw3rgFY",                                 # Уникальный ID маленькой версии фотографии. (chat.pinned_message.from_user.photo.small_photo_unique_id)
                "big_file_id": "AQADBQADAbExGw3rgFYAEAMAA8mhslkBAAPoL1uFHxdshgAEHgQ",    # ID большой версии фотографии. (chat.pinned_message.from_user.photo.big_file_id)
                "big_photo_unique_id": "AgADAb3rgFY"                                     # Уникальный ID большой версии фотографии. (chat.pinned_message.from_user.photo.big_photo_unique_id)
            }
        },
        "date": "2023-01-24 15:27:03",          # Дата отправки сообщения. (chat.pinned_message.date)
        "chat": {                               # Чат, в котором было отправлено сообщение. (chat.pinned_message.chat)
                                                # ... (информация о чате, аналогична предыдущему описанию)
        },
        "mentioned": false,                     # Флаг, показывающий, упоминается ли в сообщении текущий пользователь. (chat.pinned_message.mentioned)
        "scheduled": false,                     # Флаг, показывающий, является ли сообщение запланированным. (chat.pinned_message.scheduled)
        "from_scheduled": false,                # Флаг, показывающий, отправлено ли сообщение из запланированных. (chat.pinned_message.from_scheduled)
        "media": "MessageMediaType.PHOTO",      # Тип медиа в сообщении (в данном случае фото). (chat.pinned_message.media)
        "has_protected_content": false,         # Флаг, показывающий, содержит ли сообщение защищенный контент. (chat.pinned_message.has_protected_content)
        "has_media_spoiler": false,             # Флаг, показывающий, содержит ли сообщение спойлер медиа. (chat.pinned_message.has_media_spoiler)
        "photo": {
            "_": "Photo",                       # Тип объекта 
            "file_id": "AgACAgUAAx0CawoOYgACAUplQ1g5FZecjTHSMcKeyBXYOqQiEQACdLUxGztEeVb1sKmzMscIDgAIAQADAgADeAAHHgQ",  # ID файла фотографии (chat.pinned_message.photo.file_id)
            "file_unique_id": "AgADdLUxGztEeVY",    # Уникальный ID файла фотографии (chat.pinned_message.photo.file_unique_id)
            "width": 700,                           # Ширина фотографии (chat.pinned_message.photo.width)
            "height": 700,                          # Высота фотографии (chat.pinned_message.photo.height)
            "file_size": 72267,                     # Размер файла фотографии в байтах (chat.pinned_message.photo.file_size)
            "date": "2023-01-24 15:27:02",          # Дата отправки фотографии (chat.pinned_message.photo.date)
            "thumbs": [                             # Миниатюры фотографии (chat.pinned_message.photo.thumbs)
                {
                    "_": "Thumbnail",               # Тип объекта 
                    "file_id": "AgACAgUAAx0CawoOYgACAUplQ1g5FZecjTHSMcKeyBXYOqQiEQACdLUxGztEeVb1sKmzMscIDgAIAQADAgADbQAHHgQ",  # ID файла миниатюры (chat.pinned_message.photo.thumbs[0].file_id)
                    "file_unique_id": "AgADdLUxGztEeVY",    # Уникальный ID файла миниатюры (chat.pinned_message.photo.thumbs[0].file_unique_id)
                    "width": 320,                           # Ширина миниатюры (chat.pinned_message.photo.thumbs[0].width)
                    "height": 320,                          # Высота миниатюры (chat.pinned_message.photo.thumbs[0].height)
                    "file_size": 25406                      # Размер файла миниатюры в байтах (chat.pinned_message.photo.thumbs[0].file_size)
                }
            ]
        },
        "outgoing": false,  # Флаг, показывающий, является ли сообщение исходящим (chat.pinned_message.outgoing)
        "can_set_sticker_set": false,  # Флаг, показывающий, может ли пользователь устанавливать набор стикеров в чате (chat.can_set_sticker_set)
        "members_count": 20,  # Количество участников чата (chat.members_count)
        "permissions": {  # Права участников чата (chat.permissions)
            "_": "ChatPermissions",  # Тип объекта 
            "can_send_messages": false,  # Флаг, показывающий, могут ли участники отправлять сообщения (chat.permissions.can_send_messages)
            "can_send_media_messages": false,  # Флаг, показывающий, могут ли участники отправлять медиа-сообщения (chat.permissions.can_send_media_messages)
            "can_send_other_messages": false,  # Флаг, показывающий, могут ли участники отправлять другие типы сообщений (chat.permissions.can_send_other_messages)
            "can_send_polls": false,  # Флаг, показывающий, могут ли участники отправлять опросы (chat.permissions.can_send_polls)
            "can_add_web_page_previews": false,  # Флаг, показывающий, могут ли участники добавлять предпросмотр веб-страниц (chat.permissions.can_add_web_page_previews)
            "can_change_info": false,  # Флаг, показывающий, могут ли участники изменять информацию о чате (chat.permissions.can_change_info)
            "can_invite_users": false,  # Флаг, показывающий, могут ли участники приглашать других пользователей (chat.permissions.can_invite_users)
            "can_pin_messages": false  # Флаг, показывающий, могут ли участники закреплять сообщения (chat.permissions.can_pin_messages)
        }
}
```

![](https://ucarecdn.com/66609fd5-cca6-4940-a93a-72e6ef1cf1ce/)

Предположим ваша цель добраться до значения  `"big_photo_unique_id": "AgADAbExGw3rgFY"`, необходимо использовать следующие путь **`chat.pinned_message.from_user.photo.big_photo_unique_id`:**

Этот путь обращается к следующим элементам в этой структуре данных:

1. **`chat`**: Объект чата.
2. **`pinned_message`**: Закрепленное сообщение в чате.
3. **`from_user`**: Пользователь, отправивший закрепленное сообщение.
4. **`photo`**: Фотография профиля пользователя.
5. **`big_photo_unique_id`**: Уникальный идентификатор большой версии фотографии профиля пользователя.

Точка (`.`) в выражении `chat.pinned_message.from_user.photo.big_photo_unique_id` используется для доступа к атрибутам или свойствам объекта. Это называется "точечная нотация". Каждая точка в выражении указывает на переход к вложенному атрибуту или свойству объекта. Таким образом можно извлекать любые данные из любых объектов предоставляемые API Telegram.