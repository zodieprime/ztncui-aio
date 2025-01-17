# ztncui-aio
## ZeroTier network controller user interface in a Docker container

This is to build a Docker image that contains **[ZeroTier One](https://www.zerotier.com/download.shtml)** and **[ztncui](https://key-networks.com/ztncui)** to set up a **standalone ZeroTier network controller** with a web user interface in a container.

Follow us on [![alt @key_networks on Twitter](https://i.imgur.com/wWzX9uB.png)](https://twitter.com/key_networks)

Licensed Under GNU GPLv3

## Credit
Thanks to @kmahyyg for https://github.com/kmahyyg/ztncui-aio from which this build process is forked.

## Further information
Refer to https://github.com/key-networks/ztncui-containerized for the original documentation.

## Build yourself

```bash
$ git clone https://github.com/key-networks/ztncui-aio
$ docker build . -t keynetworks/ztncui:latest
```

Change `NODEJS_MAJOR` variable in Dockerfile to use different nodejs version.

Never use `node_lts.x` as your installation script of nodejs whose version might changed without further notice due to time shift.

## Usage

```bash
$ git clone https://github.com/key-networks/ztncui-aio # to get a copy of denv file, otherwise make your own
$ docker pull keynetworks/ztncui
$ docker run -d -p9993:9993/udp -p3443:3443 -p3180:3180 \
    -v /mydata/ztncui:/opt/key-networks/ztncui/etc \
    -v /mydata/zt1:/var/lib/zerotier-one \
    --env-file ./denv \
    --name ztncui \
    keynetworks/ztncui
```
## Usage

```
wget https://gist.githubusercontent.com/zodieprime/0f792811df503e633b90cf1cc28e1a17/raw/68e2a5a9f66689d6bf3613ea5a179c68ffbcf248/gistfile1.txt -O docker-compose.yml
docker-compose up
```

See below how to generate the `denv` file.

## Supported Configuration via persistent storage

For ZTNCUI: https://github.com/key-networks/ztncui

| REQUIRED | Name | Explanation | Default Value |
|:--------:|:--------:|:--------:|:--------:|
| YES | NODE_ENV | https://pugjs.org/api/express.html | production |
| no | HTTPS_HOST | Only Listen on HTTPS_HOST:HTTPS_PORT | NO DEFAULT |
| no | HTTPS_PORT | HTTPS_PORT | 3443 |
| no | HTTP_PORT | HTTP_PORT | 3000 |
| no | HTTP_ALL_INTERFACES | Listen on all interfaces, useful for reverse proxy, HTTP only | NO DEFAULT |

Additional environment variables used in this Docker image:

| REQUIRED | Name | Explanation | Default Value |
|:--------:|:--------:|:--------:|:--------:|
| no | MYDOMAIN | generate TLS certs on the fly (if not exists) | ztncui.docker.test |
| no | ZTNCUI_PASSWD | generate admin password on the fly (if not exists) | password |
| YES | MYADDR | your ip address, public ip address preferred | NO DEFAULT |

An example `denv` file:
```bash
NODE_ENV=production
HTTPS_PORT=3443
ZTNCUI_PASSWD=MySecret
MYDOMAIN=ztncui.docker.test
```

This image exposes an http server at port 3180, so you could save a file in `/mydata/ztncui/myfs/` to serve it. For example, you could use this to build your own root server and distribute a planet file.

**WARNING: IF YOU DO NOT SET PASSWORD, YOU HAVE TO USE `docker exec -it <CONTAINER NAME> bash`, and then `cat /var/log/docker-ztncui.log` to get your random password. This is gatekeeper.**

## Chinese users only

This script use https:///ip.sb for public IP detection purpose, which is blocked in some area of China Mainland. Under this circumstance, the program will try to detect public IP using `ifconfig` tool and might lead to unwanted result, to prevent this, make sure you set `MYADDR` environment variable when docker container is up.

The upstream repo (https://github.com/kmahyyg/ztncui-aio) only accept Issues and PRs in English. Other languages will be closed directly without any further notice. If you come from some non-English countries, use Google Translate, and state that at the beginning of the text body.

