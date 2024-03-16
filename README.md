# Docker Compose for Redmine with NGINX Reverse Proxy and Let's Encrypt - Free SSL/TLS Certificate

# Fork from glego/redmine-nginx-letsencrypt and his repository are really good and working well for me, the most of thing to get function worked, but my client need to adaption to dataabase and backup regually, So I adding a little bit for mysql part. 

`docker-compose.yml` sets up a [docker-gen][docker-gen-github], [letsencrypt-proxy-companion][docker-letsencrypt-github] and [redmine][redmine-dockerhub] in a single go. It will open the ports 80 and 433 on the host and automatically retrieve an certificate from [Let's Encrypt][letsencrypt]. Make sure to change the `VIRTUAL_HOST`, `LETSENCRYPT_HOST` and `LETSENCRYPT_EMAIL` and change the database password in mysql and redmine's section before starting the Docker containers.

## How to use

1. Clone the repository
git@github.com:jerrywang1974/redmine-nginx-letsencrypt.git
```shell
# Clone the repository
git clone https://github.com/jerrywang1974/redmine-nginx-letsencrypt

# Change into the redmine-nginx-letsencrypt directory
cd ./redmine-nginx-letsencrypt
```

2. Edit the file `docker-compose.yml` and change the `VIRTUAL_HOST`, `LETSENCRYPT_HOST` and `LETSENCRYPT_EMAIL`

3. modify `docker-compose.yml` file mysql db password
3. Start the containers by using docker-compose.

```shell
# Start Docker Containers in the foreground (attached mode)
docker-compose up

# Start Docker Containers in the background (deattached mode)
docker-compose -d up
```

## Prerequisites

Make sure that the following packages are installed and up-to-date for your operation system.

- [Docker][docker-installation]
- [Docker-Compose][docker-compose]
- (sub)domains and a publicly available web server

## Troubleshooting

- View the docker compose logs

```shell
docker-compose logs
```

- View the NGINX configuration file

```shell
docker exec -ti nginx cat /etc/nginx/conf.d/default.conf
```

- View the ports used by the containers

```
$ docker ps
CONTAINER ID   IMAGE                                    COMMAND                  CREATED          STATUS          PORTS                                                                      NAMES
29cc9c89e9fa   jrcs/letsencrypt-nginx-proxy-companion   "/bin/bash /app/entr…"   21 minutes ago   Up 21 minutes                                                                              letsencrypt-nginx-proxy-companion
d31d5176045d   jwilder/docker-gen                       "/usr/local/bin/dock…"   21 minutes ago   Up 21 minutes                                                                              nginx-gen
0666fa81898a   redmine                                  "/docker-entrypoint.…"   21 minutes ago   Up 19 minutes   0.0.0.0:32778->3000/tcp, :::32778->3000/tcp                                ctone-redmine
7017723e54b1   mysql:5.7                                "docker-entrypoint.s…"   21 minutes ago   Up 21 minutes   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp                       mysql57
2136a574e9ce   nginx                                    "/docker-entrypoint.…"   21 minutes ago   Up 21 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   nginx
```

> Under the ports column, container nginx has `0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp`, which means it's bound to the host (visible from the internet). While for redmine `3000/tcp` is only visible for the other containers.

## Notes

- I've tested the docker compose file on the following system:

```shell
$ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 9.2 (stretch)
Release:        9.2
Codename:       stretch

$ docker --version
Docker version 17.09.0-ce, build afdb6d4

$ docker-compose --version
docker-compose version 1.16.1, build 6d1ac21
```

- Some cheap VPS that supports Docker
  - [https://www.vultr.com/](https://www.vultr.com/?ref=7244423)
  - https://www.vps.ag/
  - https://www.ssdnodes.com/


## References

* https://github.com/jwilder/docker-gen
* https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion
* https://hub.docker.com/_/redmine/

[docker-gen-github]:https://github.com/jwilder/docker-gen
[docker-letsencrypt-github]:https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion
[redmine-dockerhub]:https://hub.docker.com/_/redmine/
[letsencrypt]:https://letsencrypt.org/
[docker-installation]:https://docs.docker.com/engine/installation/
[docker-compose]:https://docs.docker.com/compose/install/
Fork from glego/redmine-nginx-letsencrypt 
