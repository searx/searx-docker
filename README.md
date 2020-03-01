# searx-docker

Create a new searx instance in five minutes using Docker (see https://github.com/asciimoo/searx/issues/1561 )

This is a work in progress, the bot protection is basic and not battle tested, and later on, may [change](https://github.com/unixfox/antibot-proxy).

## What is included ?

| Name | Description | Docker image | Dockerfile |
| -- | -- | -- | -- |
| [Caddy](https://github.com/caddyserver/caddy) | Reverse proxy (create a LetsEncrypt certificate automatically) | [abiosoft/caddy:1.0.3-no-stats](https://hub.docker.com/r/abiosoft/caddy) | [Dockerfile](https://github.com/abiosoft/caddy-docker/blob/master/Dockerfile-no-stats) |
| [Filtron](https://github.com/asciimoo/filtron) |  Filtering reverse HTTP proxy, bot and abuse protection | [dalf/filtron:latest](https://hub.docker.com/r/dalf/filtron) | See [asciimoo/filtron#4](https://github.com/asciimoo/filtron/pull/4) |
| [Searx](https://github.com/asciimoo/searx) | searx by itself | [searx/searx:latest](https://hub.docker.com/r/searx/searx) | [Dockerfile](https://github.com/searx/searx/blob/master/Dockerfile) |
| [Morty](https://github.com/asciimoo/morty) | Privacy aware web content sanitizer proxy as a service. | [dalf/morty:latest](https://hub.docker.com/r/dalf/morty) | [Dockerfile](https://github.com/dalf/morty/blob/master/Dockerfile) |
| [Searx-checker](https://github.com/searx/searx-checker) | Check which engines return results of the instance.<br>JSON result available at<br>```https://{SEARX_HOSTNAME}/status```<br>Automatically updated every 24h | [searx/searx-checker:latest](https://hub.docker.com/r/searx/searx-checker) | [Dockerfile](https://github.com/searx/searx-checker/blob/master/Dockerfile) |

## How to use it
1. [Install docker](https://docs.docker.com/install/)
2. [Install docker-compose](https://docs.docker.com/compose/install/) (be sure that docker-compose version is at least 1.9.0).
3. Get searx-docker
```sh
cd /usr/local
git clone https://github.com/searx/searx-docker.git
cd searx-docker
```
4. Edit the [.env](https://github.com/searx/searx-docker/blob/master/.env) file according to your need.
5. Check everything is working: ```./start.sh```.
6. Start the containers in background: ```./start.sh -d```.

## Note on the image proxy feature

The searx image proxy is activated by default using [Morty](https://github.com/asciimoo/morty).

The default [Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) allow the browser to access to {SEARX_HOSTNAME} and ```https://*.tile.openstreetmap.org;```.

If some users wants to disable the image proxy, you have to modify [./caddy.conf.d/csp.conf](https://github.com/searx/searx-docker/blob/master/caddy.conf.d/csp.conf). Replace the ```img-src 'self' data: https://*.tile.openstreetmap.org;``` by ```img-src * data:;```

## Add directives to the Caddyfile
You may add any directives you want in the ```caddy.conf.d``` directory by creating a new file in it.
Please refer to the documentation for more details: https://caddyserver.com/v1/docs/import

## Custom docker-compose.yaml

Do not modify docker-compose.yaml otherwise you won't be able to update easily from the git repository.

It is possible to the [extend feature](https://docs.docker.com/compose/extends/) of docker-compose :
1. stop the containers : ```cd /usr/local/searx-docker && ./stop.sh```
2. create a new docker-compose-extend.yaml, check with ```./start.sh```
3. restart the containers in background : ```./restart.sh -d```

## Multi Architecture Docker images

For now only the amd64 platform is supported.

## How to update ?

Check the content of [```update.sh```](https://github.com/searx/searx-docker/blob/master/update.sh)
