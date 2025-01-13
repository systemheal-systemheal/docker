Данный проект является тестовым и используется исключительно в целях самообучения по теме docker и всего, что с ним связано.

## Создаем образы

docker build -t backend ./backend

docker build -t database ./database

docker build -t frontend ./frontend

## Создаем сеть и вольюм


docker volume create test


docker network create test_net


## Поднимаем базу данных

docker run --rm -d \
  --name database \
  --net=test_net \
  -v test:/var/lib/postgresql/data \
  -e POSTGRES_DB=docker_app_db \
  -e POSTGRES_USER=docker_app \
  -e POSTGRES_PASSWORD=docker_app \
  database

## Поднимаем бекенд

docker run --rm -d \
  --name backend \
  --net=test_net \
  -e HOST=database \
  -e PORT=5432 \
  -e DB=docker_app_db \
  -e DB_USERNAME=docker_app \
  -e DB_PASSWORD=docker_app \
  backend

## Поднимаем фронтедн

docker run --rm -d \
  --name frontend \
  --net=test_net \
  -p 80:80 \
  -v $(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
  frontend

## Заходим на сайт http://localhost
