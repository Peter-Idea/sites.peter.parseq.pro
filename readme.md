Инструкция
=========================

Проект состоит из файлов и каталогов
--------------------------

docker-compose.yml

docker-nginx1

docker-nginx2

letsencrypt

> docker-compose.yml - фаил описывающий настройки Traefik,  сайтов на базе Nginx
 для запуска в Docker Compose.

> docker-nginx1, docker-nginx2 - каталоги с содержимом сайтов, которые будут проброшены в контейнеры Nginx

> letsencrypt -  котолог с полученных сертификатом из центра сертификации Let's Encryp

Установка
----------

Для установки требуется
1. скопировать каталоги и файлы проекта в домашний катаог,
2. в каталоги docker-nginx1\html, docker-nginx2\html поместить файлы сайта
3. создать сеть proxy, которую будет слушать Traefik:

<docker network create proxy>



Добавление сайта
-----------------
Для добавления сайта(ов), например https://site3.peter.parseq.pro/,
потребуется создать каталоги docker-nginx№сайта\html (например, docker-nginx3\html)
поместить в них файлы сайта, добавить в фаил docker-compose.yml строки 
для нашего примера:

    nginx3:
    image: nginx:latest
    labels:
     - "traefik.enable=true"
     - "traefik.http.routers.nginx3.rule=Host(`site3.peter.parseq.pro`)"
     - "traefik.http.routers.nginx3.entrypoints=websecure"
     - "traefik.http.routers.nginx3.tls.certresolver=myresolver"

    networks:
    - proxy

    volumes:
     - ./docker-nginx3/html:/usr/share/nginx/html

 Если у вас не "nginx3" как в нашем примере, тогда замените на свои обозначения.
 Замените домен сайта, и путь к файлам сайта.

Запуск
------
Для запуска требуется выполнить

    docker-compose up -d
