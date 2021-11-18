# Tella Web

This repository includes the recipe for deploying a Tella Web instance (frontend and backend).

## Deployment

```docker-compose build```

During the execution it's necessary to specify what will be the secret passphrase used during the authentication process. This value cannot be changed, as it will invalidate the stored password hashes. And it must be kept secret.

```JWT_SECRET=something_secret docker-compose up -d```

_If you don't want to have to always add the environment variable to the command line you can create an .env file and store it there. docker-compose loads the variables from that file automatically. https://docs.docker.com/compose/environment-variables/#the-env-file_

## User management

To add, change permissions or list users run the following command:

```docker-compose exec api yarn console```

This will display the available subcommands. For example, if we want to add a user named superadmin and with administrator role we must execute the following:

```docker-compose exec api yarn console users create -a superadmin```

By running the list command we can see that our user superadmin is added and is an administrator (can access the web and see the reports uploaded by the users).

```docker-compose exec api yarn console users list```


