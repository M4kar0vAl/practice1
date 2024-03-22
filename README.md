# Шпаргалка по Git

## Установка

### Для Windows

1. Перейти на [официальный сайт Git](https://git-scm.com/download/win) и скачать Standalone installer для своей версии Windows
2. Запустить установщик и следовать инструкциям
3. Проверить, стоит ли галочка напротив Git Bash here - это позволит открывать консоль с Git в любой папке

### Для Linux

1. На [официальном сайте Git](https://git-scm.com/download/linux) найти команду для своей версии Linux
2. Скопировать команду в Terminal и выполнить ее

### Для MacOS

#### 1 способ

В консоли выполнить команду `/usr/bin/git`, далее следовать инструкциям установщика

#### 2 способ

1. Установить Homebrew
2. В консоли выполнить команду `brew install git`

## GitHub

### Регистрация

1. Перейти на [главную страницу GitHub](https://github.com/)
2. Нажать кнопку Sign up и следовать инструкциям
3. Нажать Create account
4. Ввести код, который придет на указанную почту

### Создание удаленного репозитория

1. Зайти в свой профиль
2. Перейти во вкладку репозитории
3. Нажать на кнопку New
4. Назвать репозиторий и выставить другие настройки при необходимости

## SSH-ключи

Для того, чтобы обезопасить обмен данными в сети использются SSH-ключи (**S**ecure **Sh**ell Protocol).  
SSH использует пару ключей:
- **приватный**
  - используется для расшифровки данных
  - хранится на компьютере пользователя
  - **НЕ ДОЛЖЕН** передаваться кому-либо еще
- **публичный**
  - используется для шифрования данных
  - доступен всем
  - данные зашифрованные этим ключом могут быть расшифрованы парным *приватным* ключом

### Проверка наличи SSH-ключей

1. Перейти в домашнюю директорию `cd ~`
2. Выполнить команду `ls -la .ssh/`, которая проверит наличие директории .ssh/ и файлов в ней
3.  
   - Если папка пустая или ее нет, то можно генерировать новый ключ
   - Если есть ключи, но вы их не создавали то нужно их удалить (примеры ключей: id_dsa.pub, id_ecdsa.pub, id_ed25519.pub, id_rsa.pub)

### Генерация ключей

1. Выполнить команду в консоли `ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"`
   - Если появлятся сообщение об ошибке, занчит скорее всего система не поддерживает этот алгоритм шифрования
   - Тогда можно воспользоваться другим алгоритмом, например: `ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"`
2. Указать место хранения ключей (оставить пустым, чтобы использовать домашнюю директорию)
3. Программа запросит создать кодовую фразу (пароль от ключа). Можно оставить пустым

### Привязывание SSH-ключа к GitHub

1. Скопировать содержимое пубичного ключа в буфер обмена
   - macOS: `pbcopy < ~/.ssh/id_ed25519.pub`
   - Windows: `clip < ~/.ssh/id_ed25519.pub`
   - Если это не срботает, то можно вывести содержимое на экран и скопировать вручную: `cat ~/.ssh/id_ed25519.pub`
2. Перейти на GitHub, выбрать пункт Settings в меню аккаунта
3. В меню слева выбрать **SSH and GPG keys**
4. В открывшейся вкладке нажать **New SSH key**
5. В поле **Title** - название ключа
6. В поле **Key type** выбрать **Authentication Key**
7. В поле **Key** вставить ключ из буфера обмена
8. Нажать кнопку **Add SSH key**
9. Проверить правильность ключа с поиощью команды `ssh -T git@github.com`
10. В предупреждении проверить ключ на сходство с [этими](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)
11. Ввести yes, если ключ совпадает, no, если не совпадает

## Хеш

**Хеширование** - способ преобразовать набор данных и получить их "отпечаток" (уникальный идентификатор).

Git хеширует информацию о коммите с помощью **алгоритма SHA-1**(Secure Hash Algorithm) и получает для каждого коммита уникальный хеш.

**Хеш** - короткая (40 символов в случае SHA-1) строка, состоящая из цифр 0-9 и латинских букв A-F.

### Свойства хеша

- Если хеш получить дважды для одного и того же набора входных данных, то результат будет гарантированно одинаковый
- Если хоть что-то в исходных данных поменяется (хотя бы один символ), то хеш тоже изменится (причём сильно)

### Хеш - основной идентификатор коммита

Git хранит таблицу соответствий `хеш -> информация о коммите` в скрытой папке `.git` в репозитории проекта.

Если известен хеш, то можно получить всю остальную информацию о коммите (автора, дату, содержимое).

Хеш можно указывать в качестве парамтера для различных Git-команд, чтобы указать, с каким коммитом нужно работать.

## Лог

**Лог** - список коммитов с описанием каждого из них.

Вывести лог можно с помощью команды `git log`

Описание коммита состоит из:
- **хеш коммита** - строка из цифр и латинских букв после слова commit
- **Author** - имя и электронная почта автора
- **Date** - дата и время коммита
- **сообщение коммита**

### Сокращенный лог

Получить сокращенный лог можно при помощи команды `git log --oneline`

В **сокращенном логе** выводятся только сообщение коммита и **сокращенный хеш** (первые несколько символов хеша).

**Сокращенный хеш** можно использовать также как и полный. Для этого команда `git log --oneline` автоматически подбирает такую длину сокращенных хешей, чтобы они были уникальными в пределах репозитория и Git всегда мог понять о каком коммита идет речь.

## HEAD

Файл `HEAD` - один из служебных файлов папки `.git`. Он указывает на последний коммит (на самый новый).

Внутри `HEAD` — ссылка на служебный файл: `refs/heads/master` (или `refs/heads/main`). В этом файле записан хеш последнего коммита.

Когда делается новый коммит, Git обновляет `refs/heads/master` - записывает в него хеш последнего коммита. При этом `HEAD` тоже обновляется, так как ссылается на `refs/heads/master`. 

`HEAD` можно передавать в Git-команды, которые принимают хеш, чтобы указать, что работать нужно с последним коммитом.

## Статусы файлов в Git

- **untracked** (неотслеживаемый)
  - этот статус имеют все новые файлы (Git видит файл, но не следит за его изменениями)
  - у untracked файлов нет предыдущих версий, зафиксированных в коммитах или через `git add`
- **staged** (подготовленный), альтернативные названия: **indexed**, **cached**
  - после выполнения комады `git add` файл попадает в *staging area*, т.е. в список файлов, которые войдут в коммит. В этот момент файл находится в состоянии *staged*.
- **tracked** (отслеживаемый)
  - это состояние противоположность **untracked**
  - в него попадают файлы, которые уже были зафиксированы с помощью `git commit`, а также файлы добавленные в *staging area* командой `git add`. Т.е. все файлы, в которых Git так или иначе отслеживает изменения.
- **modified** (измененный)
  - сюда попадают все файлы, в которых Git нашел отличия от последней сохраненной версии

Для файлов в состояниях **staged** и **modified** обычно не указывают, что они также **tracked**, потому что это состояние подразумевается.

### staged и modified

Команда `git add` добавляет в *staging area* только текущее содержимое файла. Если изменить файл, когда он уже в *staging area*, но еще не закоммичен, то новая версия файла будет иметь статус **modified**. Чтобы добавить последнюю версию в *staging area* нужно еще раз выполнить команду `git add`.

### Типичный жизненный цикл файла в Git

```mermaid
   stateDiagram
      untracked --> staged: git add
      staged --> tracked: git commit
      staged --> modified: именения
      modified --> staged: git add
      tracked --> modified: изменения
```

1. Файл только что создали. Git ещё не отслеживает содержимое этого файла. Состояние: **untracked**.
2. Файл добавили в *staging area* с помощью `git add`. Состояние: **staged** (+ **tracked**).
   - Возможно, изменили файл ещё раз. Состояния: **staged**, **modified** (+ **tracked**).
   - Обратите внимание: **staged** и **modified** у одного файла, но у разных его версий.
   - Ещё раз выполнили `git add`. Состояние: **staged** (+ **tracked**).
3. Сделали коммит с помощью `git commit`. Состояние: **tracked**.
4. Изменили файл. Состояние: **modified** (+ **tracked**).
5. Снова добавили в *staging area* с помощью `git add`. Состояния: **staged** (+ **tracked**).
6. Сделали коммит. Состояния: **tracked**.
7. Повторили пункты 4-7 много-много раз.

### `git status`

Чтобы узнать текущее состояние репозитория можно использовать команду `git status`.

`git status` подсказывает, какие команды можно выполнить, чтобы поменять состояние файла.

#### Какие состояния показывает `git status`

- **staged** (Changes to be committed в выводе `git status`);
- **modified** (Changes not staged for commit);
- **untracked** (Untracked files).

#### Варианты вывода `git status`

1. Нет ни **staged**-, ни **modified**-, ни **untracked**-файлов
   - nothing to commit, working tree clean
2. Найдены неотслеживаемые файлы
   - Untracked files: # найдены неотслеживаемые файлы<br>
  (use "git add <file>..." to include in what will be committed)
   - После `git add`:<br>Changes to be committed: # новая секция<br>
  (use "git restore --staged <file>..." to unstage)
3. Найдены изменения, которые не войдут в коммит
   - Changes not staged for commit: # ещё одна секция<br>
  (use "git add <file>..." to update what will be committed)<br>
  (use "git restore <file>..." to discard changes in working directory)
4. Файл добавлен в staging area, но после этого изменён
   - Changes to be committed:<br>
  (use "git restore --staged <file>..." to unstage)<br>
          modified:   fileA.txt<br><br>
     Changes not staged for commit:<br>
  (use "git add <file>..." to update what will be committed)<br>
  (use "git restore <file>..." to discard changes in working directory)<br>
          modified:   fileA.txt 

## Оформление сообщений к коммитам

У каждого коммита в Git есть сообщение — то, что передаётся после параметра `-m`. Например: `git commit -m "Добавить урок про оформление сообщений коммитов"`.

Есть общие рекомендации по тому, как правильно составить сообщение. Оно должно быть:
- относительно коротким, чтобы его было легко прочитать;
- информативным.

### Стили оформления

Чтобы упростить работу, команды или даже целые компании часто договариваются об определённом стиле (то есть о правилах) оформления сообщений коммитов.

Например, правила могут быть такие:
- длина сообщения от 30 до 72 символов;
- первое слово — глагол в инфинитиве («исправить», «дополнить», «добавить» и другие);
- и так далее.

#### Корпоративный

Во многих компаниях применяется *Jira* — система для организации проектов и задач. У каждой задачи в *Jira* есть идентификатор из нескольких заглавных латинских букв и номера. Например, LGS-239 значит, что это 239-я задача в проекте LGS (сокращение от англ. logistics — «логистика»).

В корпоративном стиле в начале сообщения обычно указывают *Jira-ID*, а после — *текст сообщения*.

`git commit -m "LGS-239: Дополнить список пасхалок новыми числами"`

#### Conventional Commits

Стандарт Conventional Commits (англ. «соглашение о коммитах») отличается качественной документацией и подробной проработкой. Он подходит для репозиториев с исходным кодом программ. Использовать его для других типов проектов (например, для перевода книги) было бы неудобно.

Conventional Commits предлагает такой формат коммита: `<type>: <сообщение>`. 

Первая часть `type` — это тип изменений. Таких типов достаточно много. Вот два примера:
- `feat` (сокращение от англ. feature) — для новой функциональности;
- `fix` (от англ. «исправить», «устранить») — для исправленных ошибок.

Пример: `git commit -m "feat: добавить подсчёт суммы заказов за неделю"`

Более подробно можно посмотреть по [ссылке](https://www.conventionalcommits.org/ru/v1.0.0-beta.4/#главное)

#### GitHub-стиль

GitHub можно использовать не только для хранения файлов проекта, но и для ведения списка задач (англ. issue) этого проекта. Если коммит «закрывает» или «решает» какую-то задачу, то в его сообщении удобно указывать ссылку на неё. Для этого в любом месте сообщения нужно указать `#<номер задачи>`. В таком случае GitHub свяжет коммит и задачу.

Пример: `git commit -m "Исправить #334, добавить график температуры"`

###  Инфинитив и императив

Для сообщений на **русском языке** часто рекомендуют использовать **инфинитивы**. Например: `Добавить тесты для PipkaService`, `Исправить ошибку #123` и так далее.

Для сообщений на **английском** рекомендуется использовать **повелительное наклонение** (англ. imperative). Например: `Use library mega_lib_300`, `Fix exit button` и так далее.

### Итог

Хорошо, когда:
- сообщение коммита легко читается;
- оно информативное;
- все сообщения оформлены в одном стиле.

## Как исправить коммит

Внести правки в уже сделанный коммит можно с помощью опции `--amend`: `git commit --amend`

С опцией `--amend` команда `commit` не создаст новый коммит, а дополнит последний. **При этом хеш последнего коммита изменится.**

Важно: опция `--amend` работает только с **последним коммитом (`HEAD`)**

### Дополнить коммит новыми файлами

`git commit --amend --no-edit`

Опция `--no-edit` сообщает команде `commit`, что сообщение коммита нужно оставить как было.

### Изменить сообщение коммита

`git commit --amend -m "Новое сообщение"`

При этом **хеш также изменится**, потому что изменится сообщение и время коммита.

## Как откатиться назад

### Выполнить unstage изменений

Убрать файл из **staging** поможет команда `git restore --staged <file>` (от англ. restore — «восстановить»).

Полезно, если вы создали или изменили какой-то файл и добавили его в список «на коммит» (**staging area**) с помощью `git add`, но потом передумали включать его туда.

Убрать все файлы из **staging** - `git restore --staged .`

### «Откатить» коммит

Иногда нужно «откатить» то, что уже было закоммичено, то есть вернуть состояние репозитория к более раннему.

`git reset --hard <commit hash>`

**ВАЖНО**: Все коммиты идущие после того, к которому откатываемся будут **УДАЛЕНЫ**.

### «Откатить» изменения, которые не попали ни в staging, ни в коммит

`git restore <file>`

Если случайно изменили файл, который не планировали.

Изменения в файле «откатятся» до последней версии, которая была сохранена через `git commit` или `git add`.

## Просмотр изменений в файлах

`git diff`

Эта команда поможет узнать, какие строки и в каких файлах изменились. Может быть полезно при поиске ошибок или чтобы разобраться, откуда появилась та или иная строка.

`-` - удаленные строки

`+` - добавленные строки

Выражение вида: `@@ -1,2 +1,2 @@` говорит, какие строки файла попали в сравнение. В данном случае 2 строки, начиная с 1.

### Если файл находится в modified

Команда `git diff` сравнит последнюю закоммиченную версию файла с той, что находится в состоянии **modified**.

`git diff`

### Если файл в staging area

Команда `git diff --staged` покажет изменения в **staged**-файлах относительно последних закоммиченных версий.

`git diff --staged`

### Сравнение коммитов

`git diff <commit1> <commit2>`

С помощью этой команды удобно сравнивать изменения в двух коммитах.

Здесь допускается исполльзование `HEAD` - последнего коммита

#### Порядок аргументов `git diff`

По сути команда `git diff A B` выводит список инструкций: как превратить состояние `A` в состояние `B`. Если поменять `A` и `B` местами (`git diff B A`), то и инструкции будут обратные: как превратить `B` в `A`. При этом все зелёные строки станут красными, и наоборот.

## Игнорирование файлов

1. Создать файл `.gitignore` в корне проекта
2. Указать в нем все файлы и папки, которые нужно игнорировать (можно использовать шаблоны (правила))
3. Закоммитить `.gitignore`

Правила (шаблоны) из `.gitignore` применяются только к новым (**untracked**) файлам. Если файл уже попал в **staging area** или в коммит, то правила на него не распространяются.

### Шаблоны

`#` - комментарий (не учитывается файлом `.gitignore`)

---

`<название файла/папки>` - игнорировать все файлы/папки с таким названием не только в корне репозитория, но и во всех вложенных папках

---

`*` - соответствует любой строке, включая пустую

Например:
```bash
# игнорировать все файлы, заканчивающиеся на .jpeg
*.jpeg

# игнорировать все файлы tmp во всех подпапках docs
docs/*/tmp

# игнорировать все файлы
*
```

---

`?` - соответствует любому символу

Например:
```bash
# проигнорирует fileA.txt и file1.txt. А вот файл file12.txt не будет проигнорирован, потому что в его названии два символа после file, а не один.
file?.txt
```

---

`[]` - соответствуют одному символу, указанному в скобках

Можно указывать диапазоны: [a-z], [0-9]

Например:
```bash
# проигнорирует file0.txt, file1.txt, file2.txt, но не проигнорирует file3.txt и т.д.
file[0-2].txt
```

---

`/` - указывает на каталоги. Если шаблон начинается со слеша, то Git проигнорирует файлы или каталоги только в корневой директории.

Если шаблон заканчивается слешем, то правило применится только к папке

Например:
```bash
# игнорировать todo.txt в корне репозитория
/todo.txt

# для сравнения: spam.txt будет игнорироваться во всех папках
spam.txt

# игнорировать папку build
build/ 
```

---

`**` - функция парных звёздочек (`**`) похожа на функцию одинарной (`*`). Отличие в том, как они работают с вложенными папками. Двойная звёздочка может соответствовать любому количеству таких папок (в том числе нулю). Одинарная может соответствовать только одной.

Например:
```bash
# игнорировать файлы "docs/current/tmp", "docs/old/tmp",
# а также "docs/old/saved/a/b/c/d/tmp"
# и даже "docs/tmp", потому что ноль вложенных папок тоже подходит
docs/**/tmp

# игнорировать только "docs/current/tmp" и "docs/old/tmp"
# файл "docs/old/saved/a/b/c/d/tmp" не попадает в правило
docs/*/tmp
```

Для двойной звёздочки верно то же самое, что и для одной: если задать правило `**`, то будут проигнорированы все файлы.

---

`!` - инвертирование

Например:
```bash
# игнорировать все JPEG-файлы
*.jpeg

# но только не мем с Doge
!doge.jpeg 
```

### `.gitignore` и `git status`

Игнорируемые файлы не отображаются в выводе команды `git status`, иначе они бы засоряли вывод.

Если всё же нужно отобразить все игнорируемые файлы, то это можно сделать с помощью ключа `--ignored`: `git status --ignored`. В таком случае в выводе `git status` появится раздел `Ignored files`.

## Клонирование удаленного репозитория

1. Открыть удаленный репозиторий на GitHub
2. Нажать на кнопку Code
3. Скопировать ссылку (если настроен SSH, то скопировать ссылку из вкладки SSH)
4. Перейти в папку, куда нужно клонировать репозиторий
5. Выполнить команду `git clone <скопированная ссылка>`

**Команда `git clone` автоматически связывает локальный и удалённый репозиторий.** То есть если в GitHub-репозитории что-то поменяется (например, добавятся коммиты), вам не нужно будет заново клонировать его. Достаточно будет выполнить команду, которая обновит вашу копию.

## Основные команды

`git init` - **инициализировать** репозиторий в текущей папке

`git version` - **узнать** текущую установленную версию git (можно использовать для проверки установки git)

`rm -rf .git` - **удалить** репозиторий (нужно удалить `.git`)

`git status` - проверить **текущий статус** репозитория

`git status --ignored` - вывести дополнительно и игнорируемые файлы

`git add <имя файла>` - указать какие файлы нужно **отслеживать** (для добавления всех файлов можно использовать флаг --all или . (текущая директория))

`git commit -m '<сообщение коммита>'` - **сохранить** текущие версии добавленных (при помощи `git add`) файлов

`git commit --amend --no-edit` - **дополнить ПОСЛЕДНИЙ коммит** новыми файлами  
- опция `--no-edit` указывает, что сообщение коммита менять не нужно

`git commit --amend -m "Новое сообщение"` - **изменить сообщение ПОСЛЕДНЕГО** коммита

`git remote add <имя удаленного репозитория (обычно `origin`)> <URL удаленного репозитория>` - **связать** локальный и удаленный репозиторий

`git remote -v` - **убедиться**, связаны ли репозитории

`git remote rm origin` - **отвязать** локальный репозиторий от удаленного

`git push -u origin master` - **отправить изменения** на удаленный репозиторий (первый раз)
- флаг `-u` связывает локальную ветку master с одноименной удаленной
- origin - имя удаленного репозитория
- master - название ветки

`git push` - **отправить изменения** на удаленный репозиторий

`git log` - **вывести полный лог** (список коммитов)  
Выводит:
- **хеш коммита** - строка из цифр и латинских букв после слова *commit*
- **Author** - имя и электронная почта автора
- **Date** - дата и время коммита
- **сообщение коммита**

`git log --oneline` - **вывести сокращенный лог** (помещается только первые 72 символа сообщения)  
Выводит:
- **сокращенный хеш**
- **сообщение коммита**

`git restore <file>` - **вернуть файл** к посдедней сохраненной версии

`git restore --staged <file>` - **убрать файл** из **staging**

`git reset --hard <commit hash>` - **вернуться** к состоянию репозитория на момент коммита с хешем `<commit hash>`

`git diff` - если файл в **modified**, то команда сравнит **modified** файл с последней закоммиченной версией файла.

`git diff --staged` - если файл в **staging area**, то команда покажет изменения в **staged**-файлах относительно послдних закоммиченных версий.

`git diff <commit1> <commit2>` - покажет изменения в `<commit2>` относительно `<commit1>` (покажет как из `<commit1>` сделать `<commit2>`)

`git clone <ссылка>` - клонировать удаленный репозиторий на локальную машину

---

### Команды консоли

---

#### Навигация

`pwd` - показать в какой папке сейчас находимся

`ls` - показать список файлов и папок в текущей директории

`ls -a` - показать все файлы и папки в текущей директории, включая скрытые

`cd <dir>` - сменить директорию

`cd ~` - перейти в домашнюю директорию (на windows: `c/users/user`)

`cd /` - перейти в корневую директорию

`cd ..` - перейти на уровень выше (в родительскую папку)

---

#### Работа с файлами и папками
##### Создание

`mkdir <dir>` - создать папку в текущей директории

`touch <file.txt> <file2.txt> <file3.txt> ...` - создать файл(-ы) в текущей директории

##### Копирование и перемещение

`cp <file> <destination>` - скопировать файл в папку `<destination>`

`mv <file> <destination>` - переместить файл в папку `<destination>`

##### Чтение

`cat <file>` - вывести содержимое файла в консоль

##### Запись

`echo 'text'` - выведет в консоль то, что передали в качестве параметра

`echo 'text' > file.txt` - стереть все данные в файле `file.txt` и записать туда то, что передано в качестве параметра

`echo 'text' >> file.txt` - дописать в конец файла то, что передано в качестве параметра

##### Удаление

`rm <file>` - удалить файл в текущей директории

`rmdir <dir>` - удалить папку `<dir>` (не сработает, если в папке что-то есть)

`rm -r <dir>` - удалить папку `<dir>` и все ее содержимое
