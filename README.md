# Simple Nginx Proxy in Docker with SSL

## Prerequisites

- [Docker](https://docs.docker.com/engine/installation/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Installation

1. Clone the repository.
2. Navigate to the project directory.
3. Create a network for the proxy: `docker network create proxy`
4. Start the proxy `docker-compose up -d`

To make your application (or web server) work with the proxy, you need to specify the following environment variables:

- `VIRTUAL_HOST` - The domain name of your application.
- `VIRTUAL_PORT` - The port of your application (if it differs from 80).
- `LETSENCRYPT_HOST` - The domain name of your application.
- `LETSENCRYPT_EMAIL` - Your email address.
- `LETSENCRYPT_TEST` - Set to `true` to issue a certificate for a test domain.

Additionally, your container must use the same network as the proxy (in this case, proxy).

Example:

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
    name: proxy
    external: true
```


Once configured, you will automatically receive SSL certificates, which will also be renewed automatically.
