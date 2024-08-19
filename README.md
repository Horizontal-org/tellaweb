
# Tella Web

This repository includes the recipe for deploying a Tella Web instance (frontend and backend).

## System (prefferred) requirements
This are our *preferred* system requirements, this doesn't mean that tellaweb will not work on less requirements
 - RAM: 4GB
 - DISK: 10GB 

 _if you have running tellaweb in a smaller environment we will be happy to hear about it :D_

## Prerequisites 

- docker (including the compose module)

- two domains (or subdomains), one for the admin and one for the api, the domains should be pointed to the desired server 

- an smtp server credentials

## Deploy
- first you need to clone this repo in the server you want to deploy tellaweb

- then you need to copy the .env.example and create a .env with your own credentials (credentials are explained at the .env)
> you can see what each variable means in the environment variables table

- create the docker network `docker network create net`

- on your `docker-compose.yml` you should specify what version of tellaweb you want. 
to check the latest releases you can see our [releases](https://github.com/Horizontal-org/tellaweb/releases) page.

> so for example in the `image` variable of the `api` container instead of "horizontalorg/tellaweb-api" you will put "horizontalorg/tellaweb-api:1.3.1" and the same with the `app` container image 

- after that you should pull the docker images as this:
    `docker compose pull`
- then `docker compose up -d`

- first deployment takes a few minutes, you can check the progress with `docker logs`

## Last steps 

- you need to run migrations `docker compose exec api npm run typeorm migration:run`

- and lastly create an admin user `docker compose exec api npm run console -- users create -a youruser@someemail.com`

## Environment variables table

> for passwords variables please remember to create strong passphrasaes

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
