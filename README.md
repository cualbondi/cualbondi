# Cualbondi meta repositorio

Este repositorio contiene los proyectos `api`, `apiv3`, `web`, `db` en un solo lugar orquestados con docker-compose, junto con algunos scripts utilitarios para facilitar el deploy

Para poner en marcha, instalar primero `docker-compose` y luego ejecutar

    git clone --recursive git@github.com:cualbondi/cualbondi.git
    cd cualbondi
    git submodule foreach git checkout master
    docker-compose up --build

## Migraciones (2 opciones):

1. Levantar dump de base de datos:

`cat dump.sql | docker exec -i cualbondi_db_1 sh -c "pg_restore -C -Fc -j8 | psql -U geocualbondiuser geocualbondidb"`

opcion 2 para levantar el dump si el anterior no funciona

`cat /home/jperelli/dump.sql.gz | gunzip | docker exec -i cualbondi_db_1 psql -U geocualbondiuser geocualbondidb`

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

## Update en produccion

  cd cualbondi && git pull && git submodule update && docker-compose -f docker-compose.prod.yml pull && docker-compose -f docker-compose.prod.yml up --build --force-recreate -d && service docker restart && docker-compose -f docker-compose.prod.yml restart
