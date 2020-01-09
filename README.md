# traefik-conf

## Create a Docker network

```
docker network create web
```

## run traefik

```
docker-compose up
```

## Add containers 

### From docker compose

```
version: "3.3"

services:
  my-app3:
    image: mendhak/http-https-echo
    networks:
      - web
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.my-app3.entrypoints=websecure"
      - "traefik.http.routers.my-app3.rule=(Host(`whoami.test.mithurlug.com`) && PathPrefix(`/test3`))" 
      - "traefik.http.routers.my-app3.tls=true"
      - "traefik.http.routers.my-app3.tls.certresolver=leresolver"
      - "traefik.http.middlewares.test3-stripprefix.stripprefix.prefixes=/test3" 
      - "traefik.http.routers.my-app3.middlewares=test3-stripprefix@docker" 

networks:
  web:
    external: true
```

### From CLI

```
docker run -d \
--label "traefik.enable=true" \
--label "traefik.http.routers.test-path.tls=true" \
--label "traefik.http.routers.test-path.tls.certresolver=leresolver" \
--label "traefik.http.routers.test-path.entrypoints=websecure" \
--label "traefik.http.routers.test-path.rule=(Host(\`whoami.test.mithurlug.com\`) && PathPrefix(\`/testpath\`))" \
--label "traefik.http.middlewares.test-path-stripprefix.stripprefix.prefixes=/testpath" \
--label "traefik.http.routers.test-path.middlewares=test-path-stripprefix@docker" \
--name test-path \
--network web \
mendhak/http-https-echo
```
