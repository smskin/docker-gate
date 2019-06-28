# Балансировщик для проектов
Сборка базируется на [https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion](https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion)

## Фичи
1. Перенаправляет трафик на docker сервисы
2. Автоматически генерирует и обновляет сертификаты letsencrypt 
## Логика работы
Балансировщик слушает докер сеть webproxy и автоматически добавляет доступные хосты в балансировку.
Для того, чтобы открыть доступ к сервису необходимо:
1. Добавить сервис в сеть webproxy
2. Открыть внутренний порт 80:
3. Прописать переменные VIRTUAL_HOST, LETSENCRYPT_HOST, LETSENCRYPT_EMAIL

Пример docker-compose.yml для nginx
~~~ yml
version: '2'  
services:
    nginx:  
        image: nginx:latest 
        expose:  
            - 80  
        environment:  
            - VIRTUAL_HOST="example.com"
            - LETSENCRYPT_HOST="example.com"
            - LETSENCRYPT_EMAIL="mail@example.com"
        networks:  
            - webproxy  
            - local
~~~
## Деплой
1. Установить docker-machine на хост
2. Подключите docker-machine
3. Скопируйте файл env-example в файл .env
4. Пропишите конфиг docker-machine
~~~
DOCKER_TLS_VERIFY=1  
DOCKER_HOST=
DOCKER_CERT_PATH= 
DOCKER_MACHINE_NAME=
COMPOSE_CONVERT_WINDOWS_PATHS=
~~~
5. Разверните сервис с помощью команды docker-compose up -d