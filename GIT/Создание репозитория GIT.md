Создадим в блокноте файл со следующим содержимым:

```html
<!DOCTYPE html>
<html>
<head>
<title><hello world!></title>
</head>
<body>
<h1>Hello world</h1>
</body>
</html>

```
Сохраните его в папке `\LabGit\hello` под именем `hello.html`. Этот примерный простой файл нужен для того, чтобы научиться создавать репозитории. В последствии Вы так же сможете работать со своими файлами.                          
Если Вы уже умеете работать с командной строкой, то можно в GitBashнаписать следующие команды:
```cmd
mkdir hello
cd hello
touch hello.html
```
Данные команды создают папку (каталог) hello, переходят в него и создают там html-документ с именем hello.

Открываем Git Bash в папке hello. Чтобы создать git репозиторий из этого каталога, выполните команду,
“запускающую” Git для данного репозитория:

```git
git init
```

Чтобы инициализировать репозиторий, Git создает скрытый каталог сименем.git. В этом каталоге хранятся все объекты и ссылки, которые Git использует и создает как часть истории вашего проекта.
Теперь добавим в репозиторий созданную страницу hello.html.
`git add hello.html`
`git commit -m "First Commit"`

Первая команда добавляет файлы в индекс, то есть сделает указанный файл
готовым для коммита. Вторая команда создает новый коммит с файлами из
индекса. Коммит хранит не только снимок (все индексированные файлы)
репозитория, но и имя автора со временем, что бывает полезно.
Если у вас несколько файлов, то просто перечисляем, например, git add file1
file2.

`git add . `— сделать все измененные файлы готовыми для коммита
`git add '*.txt'` — добавить только файлы, соответствующие указанному
выражению
`git add --patch filename` — позволяет выбрать какие изменения из файла
добавятся в коммит
`git reset [file] `— убрать файлы из индекса коммита (изменения не теряются)
`git diff --staged` — показать, что было добавленно в индекс с помощью git
add, но еще не было закоммиченно

Чтобы удалить файл из репозитория используют команду git rm.
Эта команда физически удалит файл с диска. Поэтому чтобы Git перестал
отслеживать файл, но он остался на диске, используют команду:

`git rm --cached имя_файла`
`git rm [file] `— удалить файл из рабочей директории и добавить в индекс
информацию об удалении
`git mv [file-original] [file-renamed]` — изменить имя файла и добавить в
индекс коммита

После создания репозитория и коммита следует проверить текущее
состояние репозитория, используя команду:

`git status`

Команда проверки состояния сообщит, что коммитить нечего. Это означает,
что в репозитории хранится текущее состояние рабочего каталога, и нет никаких
изменений, ожидающих записи.
Добавив ключ –s можно сделать вид изменений более кратким:
`git status -s`
#git