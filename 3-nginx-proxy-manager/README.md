## 1. Inside Portainer, create the network for NPM (Nginx Proxy Manager)
## 2. After that, create the stack from the compose file:
```yml
version: '3.8'

services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    volumes:
      - npm_data:/data
      - npm_letsencrypt:/etc/letsencrypt
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role == manager
      resources:
        limits:
          cpus: "0.1"
          memory: 256M
    networks:
      - npm_public
volumes:
  npm_data:
    external: true
  npm_letsencrypt:
    external: true

networks:
  npm_public:
    external: true
```