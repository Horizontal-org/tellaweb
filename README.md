
# Tella Web

This repository includes the recipe for deploying a Tella Web instance (frontend and backend).

## System (recommended) requirements
These are our *recommended* system requirements. 
 - RAM: 4GB
 - DISK: 10GB
Tella Web should still function on less resources. _If you are running Tella Web on a server with less resources than our recommended set up, we would love to hear about your experience :D_

## Prerequisites 

- Docker (including the compose module).

- Two domains (or subdomains), one for the admin and one for the api. The domains should point to the desired server.

- Credentials for an SMTP server.

## Deploy
1. Clone this repo in the server you want to deploy Tella Web.
2. Copy the .env.example and create a .env with your own credentials (credentials are explained at the .env).
   - You can see what each variable means in the environment variables table.
3. Create the docker network `docker network create net`.
4. In `docker-compose.yml`, specify what version of Tella Web you want to install. 
   - You can find the latest releases on our [releases page](https://github.com/Horizontal-org/tellaweb/releases).
   - For example: in the `image` variable of the `api` container instead of "horizontalorg/tellaweb-api", input "horizontalorg/tellaweb-api:1.3.1" and the same with the `app` container image 
5. Pull the docker images as this:
    `docker compose pull`
6. Then `docker compose up -d`
7. The first deployment will take a few minutes. You can see the progress with `docker logs`.

## Last steps 

- Run migrations `docker compose exec api npm run typeorm migration:run`

- Create an admin user `docker compose exec api npm run console -- users create -a youruser@someemail.com`

## Environment variables table

> For passwords variables, please remember to create strong passphrasaes

| Key | Description |
| --- | ----------- |
| JWT_SECRET | a secret phrase that acts as the seed for all of the jwt tokens we use |
| VIRTUAL_HOST | admin domain without any prefix |
| API_VIRTUAL_HOST | api domain without any prefix |
| COOKIE_DOMAIN | admin domain without any prefix |
| PUBLIC_DOMAIN | api domain with 'https://' prefix  |
| ADMIN_DOMAIN | admin domain with 'https://' prefix |
| MYSQL_HOST | host to connect api to db, should be 'db' |
| MYSQL_DATABASE | name of the database |
| MYSQL_USER | name of the user for mysql |
| MYSQL_PASSWORD | password of the user for mysql |
| MYSQL_ROOT_PASSWORD | password for the root mysql user |
| REDIS_PASSWORD | password for accesing redis container |
| REDIS_HOST | host to connect api to redis, should be 'redis' |
| SMTP_HOST | host of the email server you are using |
| SMTP_PORT | port for the connection, should be '25' |
| SMTP_USER | credentials of your smtp server |
| SMTP_PASS | credentials of your smtp server |
| SMTP_GLOBAL_FROM | credentials of your smtp server |
| IP_LOCATION_KEY | constant for a service we use, shoudn't be overwritten |
| NEXT_PUBLIC_MAPBOX_TOKEN | constant for a service we use, shoudn't be overwritten |
