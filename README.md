# Cualbondi meta repositorio

Este repositorio contiene los proyectos `api`, `web` y `db` en un solo lugar orquestados con docker-compose, junto con algunos scripts utilitarios para facilitar el deploy

Para poner en marcha, instalar primero `docker-compose` y luego ejecutar

    git clone --recursive -j8 git@github.com:cualbondi/cualbondi.git
    git submodule foreach git checkout master
    mv .env.example .env && nano .env
    docker-compose up --build
    docker exec -i cualbondi_api_1 manage.py migrate

Debería estar todo funcionando en `localhost:8000`

Para usar un dump de la base de datos

    cat dump.sql | docker exec -i cualbondi_db_1 sh -c "pg_restore -C -Fc -j8 | psql -U postgres"
