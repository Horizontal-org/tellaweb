
# Tella Web

This repository includes the recipe for deploying a Tella Web instance (frontend and backend).


## Prerequisites 

- docker (including the compose module)

- two domains (or subdomains), one for the admin and one for the api, the domains should be pointed to the desired server 

- an smtp server credentials

## Deploy
- first you need to clone this repo in the server you want to deploy tellaweb

- then you need to copy the .env.example and create a .env with your own credentials (credentials are explained at the .env)

- create the docker network `docker network create net`

- after that you should pull the docker images as this:
    `docker compose pull`
- then `docker compose up -d`

- first deployment takes a few minutes, you can check the progress with `docker logs`

## Last steps 

- you need to run migrations `docker compose exec api npm run typeorm migration:run`

- and lastly create an admin user `docker compose exec api npm run console -- users create -a youruser@someemail.com`