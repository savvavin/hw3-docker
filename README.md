# Docker на 8 баллов

## Описание

Python HTTP приложение со счетчиком упакованное в Docker

Используются app - Python HTTP сервер и redis - хранилище счетчика

## Настройка


скопировать файл с переменными окружения

`cp .env.example .env`

настройки:

- HOST_PORT - порт на компьютере по умолчанию 8080
- GREETING - текст приветствия

для связи с redis:

- STORAGE=redis
- REDIS_URL=redis://redis:6379/0

## запуск

`docker compose up -d --build`

проверка контейнеров

`docker compose ps`

## HTTP запросы

получить счетчик

`curl http://127.0.0.1:8080/`

увеличить счетчик

`curl -X POST http://127.0.0.1:8080/increment`

проверить работу сервера

`curl -i http://127.0.0.1:8080/healthz`

проверить доступность хранилища

`curl -i http://127.0.0.1:8080/readyz`

## хранение данных

redis хранит данные в Docker volume redis_data который подключен к /data

включена запись на диск через AOF

при команде `docker compose down` сохраняется volume
при команде `docker compose down -v` удаляются данные volume

## проверка отказоустойчивости

остановка redis

`docker compose stop redis`

когда остановили:

- GET /healthz возвращает 200
- GET /readyz возвращает 503
- POST /increment возвращает 503

запуск redis

`docker compose start redis`

redis продолжает работать без ручного перезапуска

## логи

`docker compose logs app redis`

## остановка

`docker compose down`

## отчет

проверки описаны в REPORT.md
