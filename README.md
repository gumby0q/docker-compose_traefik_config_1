
## Setup
- create docker network:
```sh
docker network create traefik_proxy
```
- configure docker network in your service (example below)
- run 
```sh
docker-compose up -d
```
---
### Examples of docker-compose service network config

```yaml
services:
  metrics_server:
    container_name: metrics_server

    networks:
      - proxy
      - default

#Docker Networks
networks:
  proxy:
    name: "traefik_proxy"
    external: true
```

### Example `Traefik` service labels

```yaml
    labels:
      - "traefik.enable=true"

      - "traefik.enable=true"
      # - "traefik.http.routers.whoami.rule=Host(`whoami.example.com`)"
      # - "traefik.http.routers.whoami.entrypoints=websecure"
      # - "traefik.http.routers.whoami.tls.certresolver=myresolver"
      - "traefik.http.routers.my-service.rule=Host(`${TRAEFIK_DOMAIN_NAME}`)"
      - "traefik.http.routers.my-service.entrypoints=web"
      - "traefik.http.services.my-service.loadbalancer.server.port=${APP_PORT}"
```
