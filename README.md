
## Setup
- create `.env` from `.env.example`
- configure variables in `.env`
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
    external:
      name: "traefik_proxy"
```

### Example `Traefik` service labels (alternative to config file `./data/custom/host.yml`)
it should work as service labeles but it didn't worked in my case :/

```yaml
    labels:
      - "traefik.enable=true"

      - "traefik.http.routers.metrics-http.rule=Host(`${TRAEFIK_DOMAIN_NAME}`)"
      # or - "traefik.http.routers.metrics-http.rule=Host(`chalova13.ddns.net`)"
      - traefik.http.routers.metrics-http.entrypoints=web
      - traefik.http.routers.metrics-http.service=metrics-http-service
      - traefik.http.services.metrics-http-service.loadbalancer.server.port=3004
    #   - "traefik.http.middlewares.force-redirect-to-https.redirectScheme.scheme=https"
    #   - "traefik.http.middlewares.force-redirect-to-https.redirectScheme.permanent=true"
    #   - "traefik.http.routers.metrics-http.middlewares=force-redirect-to-https"


      - "traefik.http.routers.metrics-https.rule=Host(`${TRAEFIK_DOMAIN_NAME}`)"
      - traefik.http.routers.metrics-https.entrypoints=websecure
      - traefik.http.routers.metrics-https.service=metrics-https-service
      - traefik.http.routers.metrics-https.tls=true
      - traefik.http.routers.metrics-https.tls.certresolver=letsEncrypt
      - traefik.http.services.metrics-https-service.loadbalancer.server.port=3004
```
