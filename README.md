# Isso

![https://hub.docker.com/r/angristan/isso/](https://img.shields.io/microbadger/image-size/angristan/isso.svg?maxAge=3600&style=flat-square) ![https://hub.docker.com/r/angristan/isso/](https://img.shields.io/microbadger/layers/angristan/isso.svg?maxAge=3600&style=flat-square) ![https://hub.docker.com/r/angristan/isso/](https://img.shields.io/docker/pulls/angristan/isso.svg?maxAge=3600&style=flat-square) ![https://hub.docker.com/r/angristan/isso/](https://img.shields.io/docker/stars/angristan/isso.svg?maxAge=3600&style=flat-square)

![](https://posativ.org/isso/_static/isso.svg)

[Isso](https://posativ.org/isso/) is a commenting server similar to Disqus, written in Python. It supports Markdown, stores comments on a SQLite database, and is used with a super light JS client.

Based on [wonderfall/isso](https://github.com/Wonderfall/dockerfiles/tree/master/isso).

This image is automatically built by [GitLab CI](https://gitlab.com/angristan/docker-isso/pipelines) and pushed to the [Docker Hub](https://hub.docker.com/r/angristan/isso/).

**I won't update this image anymore. Feel free to fork the repo.**

## Features

- Based on Alpine Linux
- Latest Isso version installed with `pip`
- Ran as an unprivileged user (see `UID` and `GID`)

### Build-time variables

- **`ISSO_VER`** : [version of Isso from PyPi](https://pypi.org/project/isso/#history) (`0.12.2`)

### Environment variables

- **`GID`** : group id *(default : `4242`)*
- **`UID`** : user id *(default : `4242`)*

### Volumes

- **`/isso/database`** : location of the SQLite database

### Port

- **`8080`**

## Usage

### Configuration

First, create a `./config/isso.conf`.

Here is an example:

```ini
[general]
dbpath = /isso/database/comments.db
host = https://isso.domain.tld/

[server]
listen = http://0.0.0.0:8080/
```

Please look at the [documentation](https://posativ.org/isso/docs/configuration/server/) for more options.

### Running the container

```sh
docker run -d \
  --name isso \
  --mount type=bind,source="$(pwd)"/database,target=/isso/database \
  --mount type=bind,source="$(pwd)"/config,target=/isso/config \
  -p 80:7777 \
  -e UID=4242 \
  -e GID=4242 \
  angristan/isso:0.12.2
```

### Docker Compose

A `docker-compose.yml` example:

```yml
version: '2.3'

services:
  isso:
    container_name: isso
    image: angristan/isso:0.12.2
    restart: always
    ports:
      - "127.0.0.1:8080:8080"
    volumes:
      - ./config:/isso/config
      - ./database:/isso/database
    environment:
     - UID=4242
     - GID=4242
```
