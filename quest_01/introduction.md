# INTRODUCTION
---
Это самый первый квест.
Твоя цель — создать папку piscinego, написать свой первый скрипт hello.sh, загрузить его в свой репозиторий и отправить на проверку.

## 1. get-ready 

| **Instruction** | **Перевод** |
|---------------------------|--------------|
|Create in your Gitea account the repository named piscinego. This repository will be the folder where all the exercises must be uploaded. Once created, clone that repository on your desktop. | Создай в своём аккаунте Gitea репозиторий с названием piscinego. Этот репозиторий будет папкой, куда нужно загружать все упражнения. После того как репозиторий создан, клонируй его на свой компьютер. |



### Тебе необходимо:
| **Действие** | **Что делать** | **Зачем** |
|---------------------------|--------------|--------------|
|*Создать репозиторий* | *На своём Gitea аккаунте создать piscinego* |*В него ты будешь загружать ВСЕ задания* |
|*Клонировать репозиторий* | *Выполнить команду git clone* |*Чтобы скачать его на компьютер* |
|*Настроить Git* | *Сохранить пароль, чтобы не вводить каждый раз* |*Удобство* |

### Команды и объяснение
- **git config --global credential.helper store** *Git будет запоминать твой логин/пароль*
- **git clone https://01.tomorrow-school.ai/git/<username>/piscinego.git** *Скачивает репозиторий на твой компьютер*

### Пример (если имя пользователя azamat):
---
git clone https://01.tomorrow-school.ai/git/azamat/piscinego.git
---
У тебя нужно написать своё имя, которое у тебя на платформе.

## 2. set 

| **Instruction** | **Перевод** |
|---------------------------|--------------|
|Once the repository is created, use your code editor to write your first shell script called hello.sh When executed, this script must print Hello choumi!, where choumi is your username.| После того как репозиторий создан, используй свой редактор кода, чтобы написать свой первый shell-скрипт с именем hello.sh. Когда этот скрипт будет запущен, он должен вывести: Hello choumi! где choumi — это твой username. |



### Тебе необходимо:
| **Что делать** | **Зачем** |
|---------------------------|--------------|
|*Создать файл hello.sh* | *Это твой первый скрипт* |
|*Написать команду, чтобы напечатать приветствие* | *Ты учишься работать с Shell Script* |

### Команды и объяснение
- **bash hello.sh** *запуск твоего скрипта*

### Пример файла hello.sh:
---
echo "Hello choumi!"
---
Вместо choumi — пиши свой username..

## 3. go-say-hello

| **Instruction** | **Перевод** |
|---------------------------|--------------|
|After that the hello.sh is executing correctly, it needs to be uploaded to the repository with the following commands: git add hello.sh git commit -m "My very first commit" git push Once these steps are applied, the file can now be submitted for grading on the platform by clicking on the "RUN INTRODUCTION TEST" button. This action will run the tests on your submitted hello.sh file.| После того как hello.sh успешно выполняется, его нужно загрузить в репозиторий с помощью следующих команд: git add hello.sh git commit -m "My very first commit" git push После выполнения этих шагов файл можно отправить на проверку на платформе, нажав кнопку "RUN INTRODUCTION TEST". Это действие запустит тесты для твоего файла hello.sh. |

### Команды и объяснение
- **git add hello.sh** *Добавляет файл в staged area*
- **git commit -m "My very first commit"** *Создаёт коммит*
- **git push** *Отправляет файл на сервер. После push файл появится в Gitea.*
