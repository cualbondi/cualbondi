# Cualbondi meta repositorio

Este repositorio contiene los proyectos `api`, `web` y `db` en un solo lugar orquestados con docker-compose, junto con algunos scripts utilitarios para facilitar el deploy

Para poner en marcha, instalar primero `docker-compose` y luego ejecutar

    git clone --recursive -j8 git@github.com:cualbondi/cualbondi.git
    git submodule foreach git checkout master
    mv .env.example .env && nano .env
    docker-compose up --build
