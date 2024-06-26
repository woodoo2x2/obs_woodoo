_**Ветка в Git**_ это подвижный указатель на один из коммитов. Обычно ветка указывает на последний коммит в цепочке коммитов. Ветка берет свое начало от какого-то одного коммита.

Во время создания проекта под управлением системы контроля версий создается ветка по умолчанию – master.

Ветка по сути — это ответвление от текущей версии проекта. Все изменения, которые будет вносить разработчик, будут сохраняться в ветке и когда функция будет полностью реализована и проверена, разработчик сможет слить все свои изменения с веткой master.

Наглядно это можно представить с помощью следующего рисунка:

![](https://ucarecdn.com/3b6262b2-d2cd-4726-80be-9dd5e3739c0c/)

На самом деле в большом проекте может быть огромное количество веток и они будут ветвиться и сливаться на протяжении всей работы:

![](https://ucarecdn.com/cdb3ba20-2594-4321-95ee-ebd3e8a3d66a/)

Допустим у нас есть 2 ветки (main и feature) и нужно перенести изменения с ветки feature на main. Это называется **слияние**.

В нашем случаи main - **целевая ветка** - та, в которую сливаем изменения, а feature - **сливаемая** - ветка, с которой нужно перенести изменения на целевую ветку. Сливаемая ветка при этом не изменяется и с ней можно продолжать работу.

Наглядно можно представить так:

![](https://ucarecdn.com/7f9fa8bb-8eb3-4ce7-a866-a8eeb99265e9/)

В Git предусмотрена команда **git merge** для реализации слияния.

А зачем нужно получать данные, если мы клонировали репозиторий со всеми изменениями, какие данные еще нужно получать?

Отвечаем: когда вы работаете в команде, то постоянно клонировать один и тот же репозиторий не имеет смысла, когда можно просто получить изменения, сделанные другим пользователем.

Для этого используют команду **git fetch**_._
#git