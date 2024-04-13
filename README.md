# Server

Docker configuration of the main server setup.

## Setup

Let's say you have a Next.js project setup, and you want to utilize this server setup. Add this repository as a submodule to your project.

```
nextjs-demo/            # <-- Your Next.js project
├── .docker/            # <-- This repository as a submodule in your project
    ├── db-data/
    ├── public/
    ├── .env
    ├── docker-compose.yml
    └── docker-wrapper
├── pages/
├── public/
├── src/
    ├── sql/                # <-- SQL files under this folder will be 
        ├── 00-create.sql   #     executed in order on the database
        ├── 01-seed.sql
├── .env
└── ...
```

## Docker submodule usage

Update to the latest:

```bash
git submodule update --init --recursive
```

Running the container. On the first run it will create the database and seed it with mocked data.

```bash
./.docker/docker-wrapper <project-name> up --build
```

If you need to recreate the database for any reason you can run each command separately. e.g.

```bash
./.docker/docker-wrapper <project-name> mariadb 'mysql -uroot -p\"$MARIADB_ROOT_PASSWORD\"' < ./src/sql/00-create.sql
```