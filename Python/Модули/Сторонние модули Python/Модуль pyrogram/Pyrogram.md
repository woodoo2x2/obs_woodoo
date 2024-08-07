[Pyrogram](https://docs.pyrogram.org/) - это современный инструмент для работы с [API Telegram](https://core.telegram.org/) обладающий подробной документацией. Pyrogram предлагает методы работы с пользовательскими аккаунтами, аналогичные тем, что используются в Telethon. Это особенно ценно для наших задач, таких как парсинг чатов Telegram. К тому же, это предоставляет нам альтернативу в ситуации, связанной с проблемами использования Telethon. Многие студенты нашего курса отметили постоянные сбои при работе с Telethon, даже когда не происходит пренебрежения установленными лимитами. Я искренне надеюсь, что после написания этого модуля по Pyrogram, подобные проблемы не начнут возникать и с Pyrogram .

## Сравнение с [[telethon]]

Pyrogram и Telethon являются двумя популярными библиотеками для работы с Telegram API на Python, но они различаются по своему подходу и функциональности, особенно в контексте парсинга данных.

Pyrogram, хотя и отличается удобством и простотой использования, работает по принципу, который может ограничивать его возможности в сборе информации из чатов, групп и профилей пользователей. Например, Pyrogram позволяет получить только первую аватарку пользователя, в то время как Telethon может извлекать любое их количество в профиле пользователя. Кроме того, Pyrogram не предоставляет доступ к описанию профиля пользователя, в отличие от Telethon, который позволяет получить эту информацию.

Pyrogram имеет преимущества, например, в работе с реакциями на сообщения. Pyrogram может получать список реакций и устанавливать их, в то время как Telethon не поддерживает работу с реакциями. Это может быть важным фактором в зависимости от специфики задачи, обычно на фрилансе это не востребовано.

С другой стороны, Telethon предлагает более широкие возможности для сбора данных, включая доступ к большему количеству информации о диалогах и сообщениях. Текущие проблемы со стабильностью работы Telethon, включая частые вылеты из учетной записи, могут существенно усложнить его использование.

Важно отметить, что ни Pyrogram, ни Telethon не могут получать информацию из вкладки "Избранное" в Telegram. Это ограничение общее для обеих библиотек и связано с конфиденциальностью перс. данных.

В целом, выбор между Pyrogram и Telethon для парсинга данных должен основываться на конкретных требованиях к функционалу и стабильности. Если важнее широкий спектр функций, Telethon может быть предпочтительнее, если решить проблемы со стабильностью. Если же приоритетом является стабильность и простота использования, Pyrogram может оказаться более подходящим выбором, несмотря на его ограничения в функциональности.
# Установка 
```
pip3 install -U pyrogram
```

Используя [TgCrypto](https://docs.pyrogram.org/topics/speedups.html) в качестве дополнительного требования (рекомендуется):

```
pip3 install -U pyrogram tgcrypto
```

## Проверка 

Чтобы убедиться, что Pygram правильно установлен, откройте оболочку Python и импортируйте ее. Если ошибка не появляется, все готово.

```
import pyrogram

print(pyrogram.__version__)
```


Прежде чем работать с API Telegram Client, необходимо получить собственный API ID и Hash. Эти параметры выдаются при регистрации в инструментах разработчика.

1. Переходим по [ссылке](https://my.telegram.org/apps) и указываем номер, который вы хотите использовать для мониторинга групп. Можно указать телефон с действующим аккаунтом в Telegram. После ввода номера телефона вам в Telegram поступит секретный код, который нужно ввести в поле "Код подтверждения".

![](https://ucarecdn.com/c025c160-3115-4268-82f0-4713d0253ae3/)

2. После того как вы ввели корректный код подтверждения и вошли в свою учетную запись Telegram, вам необходимо зарегистрировать новое приложение:

- **App title** - указываете название для вашего приложения;
- **Short name** - указываете сокращенное имя для вашего приложения;
- **URL** - можно оставить пустым;
- **Platform** **=>**
    - Выбираете "**Other (specify in description)**".
- **Description** - можно оставить пустым.

![](https://ucarecdn.com/7b81ec41-11db-4093-8f71-9b92226a79bd/)

3. Когда вы заполнили все необходимые поля и нажали на кнопку "**Create application**", сохраните все данные на этой странице у себя в блокноте. А для наших целей хватит всего двух полей: `api_id` и `api_hash`, их мы и будем использовать. После того как скопировали все данные в файл, нажимаем на кнопку "Save changes" и закрываем вкладку браузера.

![](https://ucarecdn.com/c6f91ac7-cad5-4f1e-9117-82374072bee0/)

  
**Внимание**! **Прочтите перед использованием api_id.**  
Прежде чем использовать MTProto Telegram API, обратите внимание, что все клиентские библиотеки API строго контролируются для предотвращения злоупотреблений.  
Если вы используете Telegram API для флуда, спама, подделки счетчиков подписчиков и просмотров каналов, вы будете забанены навсегда.  
В связи с чрезмерным злоупотреблением Telegram API все аккаунты, которые регистрируются или входят в систему с помощью неофициальных клиентов Telegram API, автоматически ставятся под наблюдение во избежание нарушений Условий предоставления услуг.  
Если вы не нарушали Условия предоставления услуг, но ваш аккаунт все равно забанили после использования API, напишите на recover@telegram.org, объяснив, что вы собираетесь делать с API, и попросите разблокировать ваш аккаунт.  
Обратите внимание, что письма проверяются человеком, поэтому автоматически сгенерированные письма будут обнаружены и забанены.

Когда всё установлено, можно перейти к первому запуску кода. Запустите код ниже, заменив в нём `api_id` и `api_hash` на свои.

```python
from pyrogram import Client

api_id = 2**********2
api_hash = "8*********************************7"
group_url = "python_parsing"
app = Client("my_session", api_id=api_id, api_hash=api_hash)


def main():
    with app:
        all_messages = []
        for message in app.get_chat_history(group_url, limit=100):
            all_messages.append(message.text)

        # Вывод сообщений на экран или сохранение в файл
        for msg in all_messages:
            print(msg)


main()
```

Если это ваш первый запуск, вы получите сообщение с просьбой ввести номер телефона, который вы указали при создании приложения.

```
Welcome to Pyrogram (version 2.0.106)
Pyrogram is free software and comes with ABSOLUTELY NO WARRANTY. Licensed
under the terms of the GNU Lesser General Public License v3.0 (LGPL-3.0).

Enter phone number or bot token: +7 9** НОМЕР_ТЕЛЕФОНА
Is "+7993*******" correct? (y/N): Y
The confirmation code has been sent via Telegram app
Enter confirmation code: КОД_ИЗ_ЧАТА_ТГ
```

Введите код из чата в Telegram, который вам выслали.  
![](https://ucarecdn.com/ca40f487-7a5a-4210-aeb1-e7e085c7814b/)


`Pyrogram` — это мощный и гибкий инструмент для работы с Telegram API на языке Python. Он особенно полезен для парсинга и скрапинга данных из чатов, групп и каналов. Благодаря асинхронной архитектуре, `Pyrogram` способен обрабатывать большое количество запросов и сообщений одновременно, что делает его идеальным инструментом для сбора данных в реальном времени, но в данном разделе курса, мы будем говорить только о синхронных методах, т.к. подразумевается, что не все понимают [[Асинхронность]] на должном уровне.

## Парсинг сообщений

`Pyrogram` предоставляет удобные методы для фильтрации и обработки входящих сообщений. В этом разделе курса мы будем использовать фильтры для определения типа сообщений, которые вы хотите обработать. Например, вы можете легко извлекать текст, изображения и другие медиафайлы из сообщений, а также информацию о пользователе, который отправил сообщение.

## Работа с группами и каналами

`Pyrogram` позволяет вам не только отправлять и получать сообщения в личных чатах, но и участвовать в группах и каналах. Вы можете получать уведомления о новых сообщениях, а также анализировать контент, отправляемый в группы и каналы. Это открывает широкие возможности для сбора данных и анализа тенденций в общественных чатах.