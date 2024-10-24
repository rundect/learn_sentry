# Install Sentry self-hosted 24.10.0 with https on Ubuntu 22.04

[https://develop.sentry.dev/self-hosted]

[https://github.com/getsentry/self-hosted]

## Install docker and docker compose

[https://docs.docker.com/engine/install/ubuntu]

[https://docs.docker.com/engine/install/linux-postinstall/]

## Download latest version of Sentry self-hosted

[https://github.com/getsentry/self-hosted/releases/latest]

## Install Sentry self-hosted

`./install.sh`

### Possible errors

#### bash: ./install.sh: Permission denied

Solution: change the access permissions for the following files

`chmod 744 ./install.sh`

`chmod 744 ./cron/entrypoint.sh`

`chmod 744 ./sentry/entrypoint.sh`

#### FAIL: Required minimum RAM available to Docker is 14000 MB, found 7000 MB

Solution: change ./install/_min-requirements.sh

`MIN_RAM_HARD=7000 # MB`

## Add nginx-https container to docker-compose.yaml

Changed docker-compose.yaml in this project

## Create nginx.conf for nginx-https container

Changed ./nginx-https/nginx.conf in this project

## Create ssl certificates for nginx-https container

In folder ./nginx-https create certificates

[https://webguard.pro/web-services/nginx/generacziya-ssl-sertifikata-dlya-nginx-openssl.html]

`openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout sentry.key -out sentry.crt`

In folder ./nginx-https create DH parameter file (it may take a long time)

[https://qna.habr.com/q/984533]

`openssl dhparam -out dhparam.pem 4096`

## Change ./sentry/sentry.conf.py

Changed ./sentry/sentry.conf.py in this project

`SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')`  
`USE_X_FORWARDED_HOST = True`  
`SESSION_COOKIE_SECURE = True`  
`CSRF_COOKIE_SECURE = True`  
`SOCIAL_AUTH_REDIRECT_IS_HTTPS = True`

`CSRF_TRUSTED_ORIGINS = ["https://127.0.0.1"]`

## Change ./sentry/config.yaml

Changed ./sentry/config.yaml in this project

`system.url-prefix: https://127.0.0.1`

## Builds, (re)creates, starts, and attaches to containers for a service

`docker compose up -d`
