# Docker-Compose Traefik

This is a sample configuration file for Traefik V2.
Please note that Traefik V2 is different in many ways to Traefik 1.x.

All the configuration is based on Docker labels.

This setup is ideal for a remote dev server or any server hosting several small sites.

## Example & test services

There is 2 examples services in the directory with whoami:
- inner-whoami, to test a service in the same docker-compose.yml file as Traefik.
- outer-whoami, to test a service from a different docker-compose file.

You can also see this real life example:
- [Jupyter environement with Traefik V2](https://github.com/thibaut-d/JupyterLab/blob/master/docker/python-julia-traefik/docker-compose.yml)

## .env file

The sample.env file as to be updated and renamed .env.

It handles all the environment variables.

For security reasons, it is kept out of version control.

## Commands

Create the external network. This is usefull to let services created by other docker compose files (like external-whoami) acess to Traefik.

```
docker network create traefik
```

Traefik alone:

```
docker-compose up -d traefik
docker-compose down traefik
```

Traefik + inner-whoami:

```
docker-compose -p traefik-with-whoami up -d
docker-compose -p traefik-with-whoami down
```

outer-whoami
```
docker-compose -f outer-whoami.yml -p outer-whoami up -d
docker-compose -f outer-whoami.yml -p outer-whoami down
```
