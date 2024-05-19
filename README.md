# Server

Docker configuration of the main server setup.

## Setup

Let's say you have a Next.js project setup, and you want to utilize this server setup. 
Add this repository as a submodule to your project.

```
nextjs-demo/                # <-- Your Next.js project
├── .docker/                # <-- This repository as a submodule in your project
    ├── db-data/
    ├── public/
    ├── .env                # <-- default variables
    ├── docker-compose.yml
    └── docker-wrapper
├── pages/
├── public/
├── src/
    ├── sql/                # <-- SQL files under this folder will be 
        ├── 00-create.sql   #     executed in order on the database
        ├── 01-seed.sql
├── .env                    # <-- overwrites the default variables
└── ...
```

## Docker submodule usage (nextjs-demo)

Update to the latest:

```bash
git submodule update --init --recursive
```

Running the container. On the first run, it will create the database and seed it with mocked data.

```bash
./.docker/docker-wrapper <project-name> up --build
```

If the database needs to be recreated for any reason, run each command separately. e.g.

```bash
./.docker/docker-wrapper <project-name> postgres 'psql -U ${DB_USER}' < ./src/sql/00-create.sql
```

## Default database

You can change the environment variables in the project's `.env` file. The default values are:

```bash
DB_PORT=3306
DB_ADMIN_PORT=8888
DB_NAME=project
DB_USER=project
DB_PASSWORD=secret
NODE_PORT=8080
```

## Helpful scripts

These are helpful scripts you can add to your package.json. Keep in mind that you need to replace `<project-name>` with yours. Also, this assumes you have two scripts to create and seed the database.

```json
{
  "scripts": {
    "docker:submodule": "git submodule update --init --recursive",
    "docker:build": "./.docker/docker-wrapper <project-name> build",
    "docker:up": "./.docker/docker-wrapper <project-name> up",
    "docker:down": "./.docker/docker-wrapper <project-name> down",
    "docker:ps": "./.docker/docker-wrapper <project-name> ps",
    "docker:db:init": "npm run docker:db:create && npm run docker:db:seed",
    "docker:db:create": "./.docker/docker-wrapper <project-name> postgres 'psql -U project' < ./src/sql/00-create.sql",
    "docker:db:seed": "./.docker/docker-wrapper <project-name> postgres 'psql -U project' < ./src/sql/01-seed.sql"
  }
}
```
