# Cualbondi meta repositorio

Este repositorio contiene los proyectos `api`, `apiv3`, `web`, `db` en un solo lugar orquestados con docker-compose, junto con algunos scripts utilitarios para facilitar el deploy

Para poner en marcha, instalar primero `docker-compose` y luego ejecutar

    git clone --recursive git@github.com:cualbondi/cualbondi.git
    cd cualbondi
    git submodule update --init --remote
    docker-compose up --build

Para updatear los subrepos

    git submodule update --remote

## Base de datos:

1. Unzip de la DB (solo si el dump es *.gz):

    `gunzip -c dump.sql.gz > dump.sql`

2. Importar dump en el container postgres

    2.1 Copiar el dump dentro del container

    `docker-compose ps | grep db | awk '{print $1":/tmp "}' | xargs docker cp dump.sql`

    2.2 Ejecutar el importer

    `docker-compose exec db bash import-sql.sh`

    2.3 Remover el archivo dump del container (Opcional pero recomendando)

    `docker-compose exec db rm /tmp/dump.sql`

3. Correr migraciones de django

    `docker-compose exec web python3 manage.py migrate`

## Para hacer funcionar sentry (Sólo production environments)

    docker exec -it sentry_sentry_1 sentry upgrade
    docker restart sentry_sentry_1

## Update en produccion

```
  cd cualbondi && git pull && git submodule update && docker-compose -f docker-compose.prod.yml pull && docker-compose -f docker-compose.prod.yml up --build --force-recreate -d && service docker restart && docker-compose -f docker-compose.prod.yml restart
```

## Generar dump de BD

`docker-compose exec --user postgres db bash export-sql.sh | gzip > dump-$(date --utc +%Y%m%d_%H%M%SZ).sql.gz`
