# A2Way Drop-in-Docker PHP

Drop-in Docker-based development environment starter for PHP.

## Drop-in?

Yeah, just add this package in a sub-directory of your project, do some configurations, and you are good to start your project development up with Docker.

## Requirements

- `git`
- `make`
- `docker`
- `docker-compose`

## Installation

There are 2 ways to install _A2Way Drop-in-Docker PHP_ in your project.

1. As a Git submodule.
2. Copy-n-paste into a directory.

If you choose "1", you can receive semi-automatic (You'll have to `git pull` manually.) updates from upstream (https://github.com/a2way-com/drop-in-docker-php), but you won't be able to commit any changes to it.

If you choose "2", you can make changes to it, and commit them as a part of your project, but you cannot get easy updates as in "1" choice. You can keep an eye for any updates, and manually apply them, or make this portion of the code a submodule repo you manage yourself, and pull changes from upstream to that repo.

### Installing as a Submodule.

`git submodule add https://github.com/a2way-com/drop-in-docker-php.git`

If you clone a repo with this added as a submodule, you will have to run the following command to complete pulling this and any other submodules.

- `git submodule init`
- `git submodule update`

### Copy-n-paste into a Directory

Download this repo as a ZIP, extract it, copy it, and then paste it into your project. The `drop-in-docker-php` directory should be placed as a sub-directory of your project.

## Configuring

First, open your shell's "RC File" (Eg: `~/.bashrc` and `~/.zshrc`.), and add the following line to the end of it:

```bash
export UID=<Your Linux User ID.>
```

- Make a copy of `tmp.env` as `.env`.
- Make copies of all the `tmp.*.env` files in the `env` directory. The resulting copies should have their original copies' names without the "tmp" part (Eg: `tmp.adminer.env` should be copied as `adminer.env`.).

### `.env`

| Key | Description |
| --- | --- |
| ADMINER_PORT | Port to run Adminer on. |
| APP_IMAGE | Name to tag _App_ Docker Image with. |
| APP_VOLUMES_SRC | Path to source code relative to `drop-in-docker-php/docker-compose.yml`. |
| MAILHOG_SMTP_PORT | Port for MailHog to listen for SMTP traffic. |
| MAILHOG_UI_PORT | Port for MailHog to expose UI. |
| MYSQL_PORT | Port for MySQL to listen to. |
| PROXY_IMAGE | Name to tag _Proxy_ Docker Image with. |
| PROXY_PORT | Port for Proxy to listen to HTTP traffic. |
| STATIC_VOLUMES_PUBLIC | Path to public files directory relative to `drop-in-docker-php/docker-compose.yml`. |
| UID | Your Linux User ID. |

### `env/adminer.env`

| Key | Description |
| --- | --- |
| ADMINER_DEFAULT_SERVER | Default server address. This should match the service name for MySQL in `docker-compose.yml`. |

### `env/app.env`

You can put any app specific environment variable key value pairs here.

### `env/mysql.env`

| Key | Description |
| --- | --- |
| MYSQL_ROOT_PASSWORD | Default password for `root` MySQL user. |

You can put any other environment variables for MySQL here too.

### `env/proxy.env`

| Key | Description |
| --- | --- |
| PHP_FPM_HOST | PHP FPM server address. This should match the service name for _App_ in in `docker-compose.yml`. |
| PUBLIC_PATH | Path to public files directory relative to `drop-in-docker-php/docker-compose.yml`. |
| STATIC_CONTENT_HOST | Static content server address. This should match the service name for _Static_ in in `docker-compose.yml`. |

## Using

First, install and configure _A2Way Drop-in-Docker PHP_ as described in above sections.

If your project's source directory is in the parent level of the `drop-in-docker-php` directory, set `APP_VOLUMES_SRC` variable's value to `../`. If the source directory is in the sibling level, and the source directory's name is "src", set `APP_VOLUMES_SRC`'s value to `../src/`. Be sure to replace "src" with your own source directory's name. Basically, you have to put the path to the source directory relative to the `docker-compose.yml` file.

If your project use a sub-directory of your souce directory as the public directory, set its path relative to the `docker-compose.yml` file as the value of `STATIC_VOLUMES_PUBLIC`. Also, in `env/proxy.env`, set that sub directory's path relative to the source directory as the value of `PUBLIC_PATH`. Otherwise, if your public directory is the same as your source directory, set `STATIC_VOLUMES_PUBLIC`'s value to the relative path to the source directory, and set `PUBLIC_PATH` to `./`.

Then, go inside the `drop-in-docker-php` directory, and run `make up` to start your project in Docker.

### Available `make` Commands

| Command | Description |
| --- | --- |
| down | Stops the development environment. |
| shell | Logs in to the Docker Container for the App. |
| up | Starts the development environment. |
| up-f | Starts the development environment in foreground, so you can view logs from the containers. |

## Changes Required for Common Usecases

### Running Laravel

Add the following lines to `Dockerfiles/app.Dockerfile`, just under the `RUN apk --update add shadow` line:

```
RUN apk --update add php-session
RUN apk --update add php-tokenizer
RUN apk --update add php-xml
RUN apk --update add php-dom
RUN apk --update add php-xmlwriter
RUN apk --update add php-fileinfo
```

## Extending

This is just a starter. You have to customize this to fit your project (Eg: Add more services to `docker-compose.yml` and add more commands to Dockerfiles.). You are expected to know how to work with _Docker_ and _Docker Compose_ to use this.

If you think your addition is common enough and it should be included by default, feel free to send a Pull Request to https://github.com/a2way-com/drop-in-docker-php.

## Legal

### Copyright

Copyright (c) 2021. Supun Budhajeewa <[budhajeewa.com](https://budhajeewa.com)>

### License

[MIT](./LICENSE.md)

## Show Some Support

If you like my work here, consider making a contribution :).

[<img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" width="200"/>](https://www.buymeacoffee.com/budhajeewa).
