# Cualbondi meta repositorio

Este repositorio contiene los proyectos `api`, `web` y `db` en un solo lugar orquestados con docker-compose, junto con algunos scripts utilitarios para facilitar el deploy

Para poner en marcha, instalar primero `docker-compose` y luego ejecutar

    git clone --recursive git@github.com:cualbondi/cualbondi.git
    git submodule foreach git checkout master
    mv .env.example .env && nano .env
    docker-compose up -d --build

## Migraciones (2 opciones):

1. Levantar dump de base de datos:

`cat dump.sql | docker exec -i cualbondi_db_1 sh -c "pg_restore -C -Fc -j8 | psql -U geocualbondiuser geocualbondidb"`

2. Correr migraciones de django

`docker-compose exec api python manage.py migrate`

## Usando docker-compose.dev.yml

Agregar en `/etc/hosts`

```
127.0.0.1   api.localhost
```

De esta forma la API funcionará en http://api.localhost y la web en http://localhost.


## Para hacer funcionar sentry en produccion

    docker exec -it sentry_sentry_1 sentry upgrade
    docker restart sentry_sentry_1
