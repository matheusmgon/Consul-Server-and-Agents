# Consul Server with Agents

![image](consul.agents.png)

## Project
We are using one arquiteture with consul server and thow web servers.
On this webservers have one consul agent for expose service and register on consul server catalog
Traefik is a LoadBalancer, who him use consul catalog for map new hosts and balance all loads.

### How to
Clone this project
for this project work, you need install docker and docker-compose.
```sh
curl -fsSl https://get.docker.com | sh
```
Docker-Compose Debian/Ubuntu
```sh
apt-get install docker-compose
```
Red Hat, CentOs
```sh
yum install docker-compose
```
> Note:
You need edit DNS address on all tags in config file:
 Consul/Agents/httpd/consul-agent/client.json
 Consul/Agents/nginx/consul-agent/client.json
 Consul/Server/server.json

whell.. after all tecnologies installed in yout machine, start procject
```sh
docker-compose up -d
```

>  Note:
Traefik for default not register service on Consul, so we need to register manually.
```sh
docker exec -ti traefik apk add curl
```
```sh
docker exec -ti traefik curl -XPUT -d '{"ID":"traefik","Name":"traefik","tags":["traefik.enable=true","traefik.http.routers.traefik.entrypoints=web","traefik.http.routers.traefik.rule=Host(`traefik.redelocal`)","traefik.http.routers.traefik.service=api@internal"],"port":80}}' http://consul-server:8500/v1/agent/service/register?replace-existing-checks=true
```