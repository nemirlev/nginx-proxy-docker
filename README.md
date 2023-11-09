# Простой Nginx прокси в Docker с SSL

## Предварительные требования

- [Docker](https://docs.docker.com/engine/installation/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Установка

1. Склонируйте репозиторий
2. Перейдите в папку с проектом
3. Создайте сеть для прокси `docker network create proxy`
4. Запустите `docker-compose up -d`

Для того что бы ваше приложение (или веб-сервер) работал, требуется указать дополнительные переменны:

- `VIRTUAL_HOST` - доменное имя вашего приложения
- `VIRTUAL_PORT` - порт вашего приложения, указывается в случае если он отличается от 80
- `LETSENCRYPT_HOST` - доменное имя вашего приложения
- `LETSENCRYPT_EMAIL` - ваш email
- `LETSENCRYPT_TEST` - если установить значение `true`, то сертификат будет выдан для тестового домена

Помимо этого, в вашем контейнере требуется использовать сеть, в которой крутиться прокси (в данном случае `proxy`).

Пример:

```yml
version: '3'

services:
  app:
    image: nginx
    environment:
      VIRTUAL_HOST: example.com
      LETSENCRYPT_HOST: example.com
      LETSENCRYPT_EMAIL:
    volumes:
        - ./app:/var/www/html
    networks:
        - proxy

networks:
    proxy:
        external:
          name: proxy
```


После этого вы автоматически получите сертификаты, которые так же будут обновляться автоматически.