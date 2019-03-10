# PhpStorm development with [Docker](https://github.com/docker/docker-ce)

Development environment for PhpStorm based on Docker, managed with [Compose](https://github.com/docker/compose).

## Installation

### Requirements

* Docker >= 1.8.3
* Docker Compose

See [Install Docker Compose](https://docs.docker.com/compose/install/) for more information.

### Setup

Clone this repository into `~/PhpstormProjects/docker-dev-phpstorm`.

#### Configuration

You may need to configure your project and volume paths in `docker-compose.yml`.

The project default path is set to `~/PhpstormProjects`.
The data default path is set to `~/PhpstormProjects/docker-dev-phpstorm/data`.

##### nginx

Create the file `nginx-projects.conf` (or any other `*.conf` file) in your `nginx` folder and insert your server
configurations.   
The file(s) will be copied into the nginx `conf.d` directory.

##### node

In order for nginx to run, a default `server.js` in the `node8` directory is executed on startup.   
You can change the node entry point in the `node8/Dockerfile`.

#### Build

Run `docker-compose build` to fetch and build all containers.

After your containers are built, run `docker-compose up -d` and start coding!

## Containers

| Container   | Name    | Port  | Version  |
| ----------- | ------- | ----- | -------- |
| nginx       | nginx   | 8080  | 1.9.*    |
| node        | node8   | 9008  | 8.*      |
| mysql       | mysql57 | 9017  | 5.7.*    |
| php (fpm)   | php56   | 9056  | 5.6.*    |
|             | php71   | 9071  | 7.1.*    |
|             | php72   | 9072  | 7.2.*    |
|             | php73   | 9073  | 7.3.*    |

## Data persistance

MySQL data is stored in the `data` directory.

## File and folder permissions

Sadly, permissions are a bit of pain right now. Most containers write data with their own `www-data` users. Data written
by this user can not be accessed by your host user. You will have to `chown` files created by your containers.

It's the same the other way around, containers will not be able to write data in your project directory, unless you
grant them access. This can be done by granting read and write access on your projects folder to everyone. Please keep
in mind that this is a security risk!

Please let me know if there is an easy solution for this. Current solutions are too hacky.

## Help

### FAQ
* [How to setup Piwik with PHP 5.6 and MySQL 5.6](https://github.com/jaylinski/docker-dev-phpstorm/wiki/FAQ#how-to-setup-piwik-with-php-56-and-mysql-56)
* [How can I see error logs from PHP?](https://github.com/jaylinski/docker-dev-phpstorm/wiki/FAQ#how-can-i-see-error-logs-from-php)

### Useful commands

#### Start
`$ docker-compose up -d`

#### Stop
`$ docker-compose stop`

#### List containers
`$ docker-compose ps`

#### Open container bash
`$ docker-compose exec nginx /bin/bash`

#### Remove all docker containers and images
> Do only execute if you know what you are doing!

`$ docker rm $(docker ps -a -q)`   
`$ docker rmi -f $(docker images -q)`
