# Introduction

This repository is a fork of the [js-docker](https://github.com/TIBCOSoftware/js-docker) repository that has been updated to include support for building,
configuring, and running **JasperReports Server (Community Edition)**, **PostgreSQL** and **pgAdmin** in containers.

### Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop)

## Clone the project

Change the current working directory to the location where you want the cloned directory to be:

```
cd ~/workspace
```

Clone the project by running the following command:

```
git clone git@github.com:Robinyo/js-docker.git
cd js-docker
```

[Download](https://sourceforge.net/projects/jr-community-installers/files/Server/) the Community Edition of
JasperReports Server and place it in the `resources` directory.

Then run the following commands:

```
cd resources
chmod 755 unpackWARInstaller-ce.sh
./unpackWARInstaller-ce.sh
```

[Download](https://github.com/fg2it/phantomjs-on-raspberry/releases/) the ARM 64-bit version of PhantomJS and place it in the `resources` directory. </br>
Alternatively, [download](https://phantomjs.org/download.html) the AMD 64-bit version of PhantomJS and place it in the `resources` directory.

[Download](https://jdbc.postgresql.org/download/) the PostgreSQL JDBC driver and place it in the `resources`
directory.

# Build the project

To build the project:

```
# cd ~/workspace/js-docker

docker system prune && \
docker container prune && docker volume prune && docker network prune

export DOCKER_DEFAULT_PLATFORM=linux/arm64
# export DOCKER_DEFAULT_PLATFORM=windows/amd64

docker compose build
```

## Serve the applications

To run a multi-container application with the Docker CLI, you use the `docker compose up` command.
This command uses the project's [docker-compose.yml](https://github.com/Robinyo/js-docker/blob/master/docker-compose.yml)
file to deploy a multi-container application:

```
docker compose up -d
```

**Note:** The JasperReports Server, PostgreSQL and pgAdmin containers may take a minute or two to startup.

Navigate to the JasperReports Server Community Edition welcome page: http://localhost:11001/jasperserver

You can login using the following credentials:
* JasperReports Admin User - User ID: `jasperadmin` and Password: `jasperadmin`
* Sample User - User ID: `joeuser` and Password: `joeuser`

<p align="center">
  <img src="https://github.com/Robinyo/js-docker/blob/master/docs/screen-shots/login.png">
</p>

To stop the services:

```
docker compose stop
```

To remove the services and the associated data, run:

```
docker compose down -v
```

Note: The `-v` flag deletes all volumes, including process data, users, and other persisted state. Omit `-v` if you want to keep your data.

To check the environment variables inside your container:

```
docker inspect -f \
  '{{range $index, $value := .Config.Env}}{{println $value}}{{end}}' \
  serendipity-bff
```

You can check the status of the containers using the following command:

```
docker compose ps
```

To check the logs inside a container:

```
docker container logs postgres
docker container logs pgadmin
docker container logs jasperreports-server
docker container logs jasperreports-server-cmdline
```
