# Что было сделано
Добавлен docker-compose файл для развертывания сервиса баз данных и веб сервера, под них добавлена в проект конфигурационная папка "service".

Локально тестово сервис запустился, в процессе запустил виртуальное окружение python3.11 с инсталяцией poetry и добавлением в него django. В контейнере запустить не удалось, уперся в то, что сервис при старте не находит базу данных, решить этот кейс не успел.

# Описание тестового сервиса

В этом репозитории тестовый сервис для Devops кандидатов и тестовое задание.

Для управления зависимостями используется пакет poetry. Он позволяет управлять виртуальными окружениями и устанавливать в них пакеты. Так же позволяет запускать команды внутри виртуального окружения. Запуск через Poetry используется только в среде разработка, внутри docker контейнера все зависимости ставятся глобально и команды выполняются без использованя виртуальных окружений и poetry.

## Создание виртуального окружения
Выполниь команду:
`poetry install`
Она создаст виртуальное окружение и установит все необходимые пакеты

## Запуск сервиса
Все команды `poetry run...` необходимо выполнять из каталога `devops_demo` там где лежит файл `manage.py`

Перед тем как запустить сервис, необходимо подготовить базу данных. Для этого надо сгенерировать миграции:
`poetry run python manage.py makemigrations`
И выполнить их:
`poetry run python manage.py migrate`
после этих команд в базе данных будут созданы все необходимые таблицы.
Для запуска сервиса локально в виртуальном окружении выполнить команду:
`poetry run python manage.py runserver 0.0.0.0:8000`
Для создания пользователя в консоли после всех минраций надо выполнить команду:
`poetry run python  manage.py createsuperuser`
После чего в интерактивном режиме ввести все запрашиваемые данные.

## Запуск тестов в виртуальном окружении
Для запуска тестов локально в виртуальном окружении выполнить команду:
`poetry run python manage.py test`
Эта команда выполнит все тесты в проекте

## Требования
Для запуска сервиса и тестов необходима база данных Postgres. Ее можно поднять локально или внутри docker. Для того, что бы передать сервису параметры подключения к базе данных можно использовать переменные окружения. Например:
```
DB_NAME=postgres
DB_USER=postgres
DB_PASSWORD=postgres
DB_HOST=127.0.0.1
DB_PORT=5432
```
В примере указаны значения по умолчанию.


# Задание
1. Надо подготовить docker-compose.yml файл, который позволит собирать и запускать базу данных Postgres, выполнять все необходимые миграции и запускать сервис. Как результат этого шага  я должен иметь возможность выполнить команду `docker-compose build` для сборки свежего образа сервиса. И `docker-compose up` для запуска сервиса. Посдле запуска в браузере по адресу http://localhost:8000/admin я должен увидить страницу с формой ввода логина и пароля. Порт на котором будет запущен сервис можете выбрать на ваше усмотрение.
2. Надо реализовать CI процесс в **Github** для запуска тестов и в случае их успешного прохождения выполнить сборку docker образа. Если же тесты не прошли весь CI процесс должен падать с ошибкой.

### Усложнения
3. В качестве усложнения первого задания, запустить сервис с испоьзованием Nginx и созданием пользователя при старте docker-compose. В этом случае в форме ввода логина и пароля я смогу ввести данные и перейти в панель администратора.
4. В качестве усложнения задания 2 настроить тегирование собираемого образа и задеплоить его в docker-hub (https://hub.docker.com/) в открытый репозиторий. В нем доступна бесплатная загрузка одного образа. Этого дотаточно для тестового задания.

Для части задания с docker-compose.yml лучше форкнуть текущий репозиторий и положить файл туда. Так проще выполнять задание и будет возможность легко проверить как вы его выполнили.
Для выполнения CI можно форкнуть текущий репозиторий и все выполнить в нем. Или сделать свой репозиторий, который будет выгружать текущий и выполнять CI. Это решение остается за исполнителем. 

В форкнутом репозитории в README.md описать инструкции как запускать docker-compose. И все шаги, которые необходимо выполнить, для тестирования выполненого задания. Если такие шаги потребуются. 
