# docker-lsyncd

![CI](https://github.com/RealOrangeOne/docker-lsyncd/workflows/CI/badge.svg)

Docker container for `lsyncd`.

## Usage

The container will start `lsyncd` on startup, and begin trying to sync however is configured. The default configuration file is `/config/lsyncd.lua`, which will need mounting in.

### Authentication

Rsync uses ssh by default, and the easiest way to do this is using keys. The default key location should be `/config/.ssh/id_rsa{,.pub}`.

If you receive the message "Host key verification failed.", You'll need to copy the host key from another machine, and mount a file for `/config/.ssh/known_hosts`.

## docker-compose

```
 lsyncdd:
    container_name: lsyncd
    image: tiagoqpinto/lsyncd
    restart: always
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ${DOCKER_CONFIG_PATH}/lsyncd:/config
      - /mn/data_source:/source
      - /mnt/data_target:/target
```

```
lsyncd.lua
sync {
    default.rsync,
    source = "/source",
    target = "/target",
}
```

## developer

For the developer, build and push commands:

```
docker build -t tiagoqpinto/lsyncd:latest -t tiagoqpinto/lsyncd:v1.0.0 .
docker image push --all-tags tiagoqpinto/lsyncd
```
