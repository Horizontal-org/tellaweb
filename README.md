
# Tella Web

Tella Web is an open-source tool that enables individuals and organizations to centralize and manage reports sent by Tella users, including photos, videos, and audio files.

This repository covers instructions for deploying a Tella Web instance (frontend and backend).

> This documentation only covers instructions to deploy a Tella Web instance. To learn about Tella features [see here](https://tella-app.org/docs) and to learn about how to manage and operate your Tella Web instance [see here](https://tella-app.org/tella-web/#set-up-your-project-on-your-server)

## System requirements
*Minimum* system requirements: 
 - RAM: 4GB
 - DISK: 10GB
> For disk space, the data stored in Tella Web is in the same server, so consider monitoring server storage and adding space if needed. Tella Web should still function on less resources. _If you are running Tella Web on a server with less resources than our recommended set up, we would love to hear about your experience :D_

*Recommended* system requirements:
Ideal disk space depends on how much data you intend to collect. A good role of thumb would be to allocate the double of the space that users would submit to your server. When monitoring, we recommend for you to aim to have the same amount of free and used space in the server.

We don't have any particular recomendation about hosting providers for Tella Web: this is a pretty straightforward application so it should work anywhere you can install Docker. If you experience any issues please [contact us](https://tella-app.org/contact-us).

## Prerequisites 

- Docker (including the compose module).

- Two domains (or subdomains), one for the admin and one for the api. The domains should point to the desired server.

- Credentials for an SMTP server.

## NGINX Configuration

To make Tella Web ready to use out of the box, we added to the Docker Compose file an nginx container and a Let's Encrypt container that creates a ssl certificate for the domains. As a result, by default, the site is served in the :443 port.

To use a different nginx installation or use custom ports, please install and configure nginx manually. However, please note that we have configured Tella Web's default nginx configuration to improve files streaming and that these improvements will be lost in a manual nginx setup. 

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

- Finally, you can [read here](https://tella-app.org/tella-web/#set-up-your-project-on-your-server) about finalizing the setup, configuring the instance, and Tella Web features.


## Updating and Maintainance
We recommend monitoring our [releases page](https://github.com/Horizontal-org/tellaweb/releases) and updating Tella Web when possible to ensure you get the latest security improvements and new features. 

To update Tella Web, we recommend:

1. Doing a backup of the database.

2. Changing the image name with the release number you want to use both for the api and app containers.

3. Pulling and upping the new images.

4. Running migrations with `npm run typeorm migration:run` inside the api container or as a one line command `docker compose exec api npm run typeorm migration:run`.

5. Checking the logs with `docker logs` to ensure that everything is working properly.

6. Removing old images to optimize space with `docker prune`.

## Environment variables table

> For passwords variables, please remember to create strong passphrasaes

| Key | Description | Notes |
| --- | ----------- |-------|
| JWT_SECRET | a secret phrase that acts as the seed for all of the jwt tokens we use | required |
| VIRTUAL_HOST | admin domain without any prefix | required |
| API_VIRTUAL_HOST | api domain without any prefix | required |
| COOKIE_DOMAIN | admin domain without any prefix | required |
| PUBLIC_DOMAIN | api domain with 'https://' prefix  | required |
| ADMIN_DOMAIN | admin domain with 'https://' prefix | required |
| MYSQL_HOST | host to connect api to db, should be 'db' | required |
| MYSQL_DATABASE | name of the database | required |
| MYSQL_USER | name of the user for mysql | required |
| MYSQL_PASSWORD | password of the user for mysql | required |
| MYSQL_ROOT_PASSWORD | password for the root mysql user | required |
| REDIS_PASSWORD | password for accesing redis container | required |
| REDIS_HOST | host to connect api to redis, should be 'redis' | required |
| SMTP_HOST | host of the email server you are using | optional |
| SMTP_PORT | port for the connection, should be '25' | optional |
| SMTP_USER | credentials of your smtp server | optional  |
| SMTP_PASS | credentials of your smtp server | optional  |
| SMTP_GLOBAL_FROM | credentials of your smtp server | optional |
| IP_LOCATION_KEY | constant for a service we use, shoudn't be overwritten | optional  |
| NEXT_PUBLIC_MAPBOX_TOKEN | constant for a service we use, shoudn't be overwritten | optional |

- The [Suspicious Login detection feature](https://tella-app.org/tella-web#admin-center) won't work if the SMTP server and the ip location services are not working. If won't use those services you need to turn off the feature on the Tella Web Admin Center.

