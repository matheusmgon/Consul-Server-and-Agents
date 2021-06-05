# Consul Server with Agents

![image](consul-agents.png)

## Project

We are using a simple architecture, one consul server and two web servers (apache and nginx). The webservers run a consul agent in order to expose and register their services into the consul server, which will be a catalog service.

Traefik runs as a LoadBalancer, it will use the consul catalog to get a map of new hosts and also to balance all workloads.

### How to run

Clone this repo git and first of all you need to install the prerequisites tools: docker and docker-compose.

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
You need to edit DNS address on all tags in config file:
 Consul/Agents/httpd/consul-agent/client.json
 Consul/Agents/nginx/consul-agent/client.json
 Consul/Server/server.json

So, after all prerequisites are installed, launch the project

```sh
docker-compose up -d
```

>  Note:
For default traefik doesn't register its service automatically into consul, so we have to do manually.

```sh
docker exec -ti traefik apk add curl
```

```sh
docker exec -ti traefik curl -XPUT -d '{"ID":"traefik","Name":"traefik","tags":["traefik.enable=true","traefik.http.routers.traefik.entrypoints=web","traefik.http.routers.traefik.rule=Host(`traefik.redelocal`)","traefik.http.routers.traefik.service=api@internal"],"port":80}}' http://consul-server:8500/v1/agent/service/register?replace-existing-checks=true
```
