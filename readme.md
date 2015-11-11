# PhpStorm development with [Docker](https://github.com/docker/docker)

Development environment for PhpStorm based on Docker, managed with [Compose](https://github.com/docker/compose).

## Installation

### Requirements

* Docker >= 1.8.3
* Docker Compose

See [Install Docker Compose](https://docs.docker.com/compose/install/) for more information.

### Configuration

You may need to configure your project and volume paths in `docker-compose.yml`.

The project default path is set to `~/PhpstormProjects`.
The data default path is set to `~/PhpstormProjects/.docker/data`.

#### nginx

Create the file `nginx-projects.conf` (or any other `\*.conf` file) in your `ngninx` folder and insert your server
configruations.   
The file(s) will be copied into the nginx `conf.d` directory.

### Setup

Clone this repository into `~/PhpstormProjects/.docker` and run `docker-compose build`.

After the containers are built, run `docker-compose up -d` and start coding!

## Containers

| Container   | Port  | Version  |
| ----------- | ----- | -------- |
| nginx       | 8080  | 1.9.*    |
| node.js*    | 8090  | 4.2.*    |
| php-fpm     | 9054  | 5.4.*    |
| php-fpm     | 9055  | 5.5.*    |
| php-fpm     | 9056  | 5.6.*    |
| php-fpm     | 9070  | 7.0.*    |
| mysql       | 9016  | 5.6.*    |
| mysql       | 9017  | 5.7.*    |

\* coming soon

## Data persistance

MySQL data is stored in the `data` directory.

## File and folder permissions

Sadly, permissions are a bit of pain right now. Most containers write data with their own `www-data` users. Data written
by this user can not be accessed by your host user. You will have to `chown` files created by your containers.

It's the same the other way around, containers will not be able to write data in your project directory, unless you
grant them access. This can be done by granting read and write access on your projects folder to everyone. Please keep
in mind, that this is a security risk!

Please let me know if there is an easy solution for this. Current solutions are too hacky.

## Useful commands

### Start
`$ docker-compose up -d`

### Shut down
`$ docker-compose kill`

### List containers
`$ docker-compose ps`

### Open container bash
`$ docker-compose run nginx /bin/bash`

### Remove all docker containers and images
> Do only execute if you know what you are doing!

`$ docker rm $(docker ps -a -q)`   
`$ docker rmi $(docker images -q)`

## [FAQ](https://github.com/jaylinski/docker-dev-phpstorm/wiki/FAQ)

* How to setup Piwik with PHP 5.6 and MySQL 5.6
