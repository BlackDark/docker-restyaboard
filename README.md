Docker Restyaboard
===================

Trello like kanban board. http://restya.com/board/
Github Restyaboard: https://github.com/RestyaPlatform/board

TL;DR
-----

```bash
git clone https://github.com/blackdark93/docker-restyaboard
cd docker-restyaboard
git checkout alpine
docker-compose up -d
```
http://127.0.0.1:1234
```
Username: admin
Password: restya
```

docker-compose.yml
------------------
```
version: '2'
services:
  restyaboard:
    image: blackdark93/docker-restyaboard:latest
    ports:
      - 1234:80
    volumes:
       - /var/opt/restya/media:/usr/share/nginx/html/media
    environment:
      - POSTGRES_HOST=postgres
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=admin
      - POSTGRES_DB=restyaboard
    depends_on:
      - postgres
    restart: always
  postgres:
    image: postgres
    ports:
      - 5432:5432
    environment:
      - POSTGRES_HOST=postgres
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=admin
      - POSTGRES_DB=restyaboard
    restart: always
```


Default users
-------------
```
http://(ServerIP):1234

Username: admin
Password: restya

Username: user
Password: restya
```


Build from github
-----------------

Build image and Run containers using docker-compose.

``` bash
git clone https://github.com/blackdark93/docker-restyaboard.git
cd docker-restyaboard
```

Edit docker-compose.yml and modify the environment variables to suit your needs.

There are default values for the SMTP configuration,
so you can delete those SMTP_* variables if you don't need this feature.

``` bash
docker-compose up -d
```

Please wait a few minutes to complete initialize.


Change Restyaboard Version
--------------------------

Edit Dockerfile.
Available version on https://github.com/RestyaPlatform/board/releases

```
ENV RESTYABOARD_VERSION=REPLACE_ME
```

In case of upgrade version, rebuild image and recreate container.

```sh
docker-compose build
docker-compose up -d
```

If you want to upgrade database, e.g.
(recommend to backup database before upgrade)

```sh
docker-compose run --rm restyaboard bash

export PGHOST=${POSTGRES_HOST}
export PGPORT=5432
export PGUSER=${POSTGRES_USER}
export PGPASSWORD=${POSTGRES_PASSWORD}
export PGDATABASE=${POSTGRES_DB}
...
psql -f sql/upgrade-0.4.2-0.4.3.sql
psql -f sql/upgrade-0.4.3-0.4.4.sql
...
exit
```


Problems
------------------------------
In our company we use Ubuntu 14.04 and have central servers.
In this environment the compose file is not working properly not sure why.
After starting the container you have to bash into the restyaboard container:
Starting from the working dir execute the following:
```
chown -cR www-data:www-data .
chmod -R 0755 .
touch tmp/cache/client.php
touch tmp/cache/site_url_for_shell.php
```

This is only a __temporary__ solution!

License
------------------------------

[OSL 3.0](LICENSE.txt) fits in Restyaboard.
