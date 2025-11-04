# traefik
A simple traefik config ready to use

## Requirements
You just need Docker and Docker Compose

## Usage
1. Add your <email> in traefik.yml for acme challenges
2. To start Traefik: `docker compose up -d`
3. To use Traefik in other services add the following `labels` in your `docker-compose.yml` (replacing yourservicename appropriately). 
   ```
       labels:
        - "traefik.enable=true"
        - "traefik.http.routers.<yourservicename>.rule=Host(`example.com`)"
        - "traefik.http.routers.<yourservicename>.entrypoints=web"
        - "traefik.http.routers.<yourservicename>-secure.rule=Host(`example.com`)"
        - "traefik.http.routers.<yourservicename>-secure.entrypoints=websecure"
        - "traefik.http.routers.<yourservicename>-secure.tls.certresolver=letsencrypt"
        - "traefik.http.services.<yourservicename>.loadbalancer.server.port=8000"
        - "traefik.http.middlewares.redirect-to-https.redirectscheme.scheme=https"
        - "traefik.http.routers.<yourservicename>.middlewares=redirect-to-https"
   ```
And networks:
  ```
      networks:
        - traefik
      restart: unless-stopped
  networks:
    traefik:
      external: true
  ```
