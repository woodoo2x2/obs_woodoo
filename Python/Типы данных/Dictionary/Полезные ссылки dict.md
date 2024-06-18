Послушать о текущей реализации [[Тип данных dict]] можно по [ссылке](https://www.youtube.com/watch?v=37S53yFg9wc).

 У обычных словарей (тип `dict`) есть методы `pop()` и `popitem()`, о которых можно почитать по [ссылке](https://stepik.org/lesson/446696/step/1?unit=437002).
 
Словари в Python нередко модифицируются. Они становятся быстрее и эффективнее работают с памятью, однако основная идея их внутреннего устройства — хеширование — остается неизменной. Видео с подробным рассказом о внутреннем устройстве словарей и их эволюции от Реймонда Хеттингера, разработчика ядра Python, доступно по [ссылке](https://www.youtube.com/watch?v=37S53yFg9wc&t=15s&ab_channel=IgorStarikov). Слайды, используемые в выступлении, доступны по [ссылке](https://stepik.org/media/attachments/lesson/903604/Pycon2017CompactDictTalk.pdf). Видео переведено и озвучено на русский язык, поэтому настоятельно рекомендуется к просмотру.

 Исходный код типов `dict` и `set` доступен по [ссылке](https://github.com/python/cpython/blob/main/Objects/dictobject.c) и [ссылке](https://github.com/python/cpython/blob/main/Objects/setobject.c).