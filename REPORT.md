# отчет

выполнены задания на 8 баллов

## 1. (4 балла)

сборка

`docker build -t hw3-web:1 .`

запуск

`docker run -d --name hw3 -p 127.0.0.1:8080:8000 -e GREETING="Hello from Docker!" -e STORAGE=sqlite hw3-web:1`

выполнены GET / и POST /increment

логи приложены в logs-4.txt

## 2. (6 баллов)

проверены SQLite и именованный volume counter_data

выполнены команды:

`docker compose up -d --build`
`curl -X POST http://127.0.0.1:8080/increment`
`docker compose down`
`docker compose up -d`
`curl http:127.0.0.1:8080/`

счетчик сохраняется после удаления контейнера

## 3. (8 баллов)

итоговая конфигурация содержит app и redis

используется STORAGE=redis и REDIS_URL=redis://redis:6379/0

redis сохраняет данные в volume redis_data с AOF

для обоих сервисов настроен healthcheck

## сохранение счетчиков

выполнены несколько POST /increment

далее

`docker compose down`
`docker compose up -d`
`curl http://127.0.0.1:8080/`

значение счетчика сохранилось

## отключение redis

`docker compose stop redis`

далее проверены

`curl -i http://127.0.0.1:8080/healthz`
`curl -i http://127.0.0.1:8080/readyz`
`curl -i -X POST http://127.0.0.1:8080/increment`

## восстановление redis

`docker compose start redis`

затем выполнены

`curl http://127.0.0.1:8080/`
`curl -X POST http://127.0.0.1:8080/increment`

после восстановления счетчик сохранился и инкремент работает
вручную приложение не перезапускалось

## логи

логи по каждому пункту хранятся в
logs-4.txt
logs-6.txt
logs-8.txt

