## 1. Inside Portainer, create the stack from the compose file:
```yml
version: '3'

services:
   wp_db:
     image: mysql:5.7
     volumes:
       - wp_db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
     networks:
     - npm_public
   wordpress:
     depends_on:
       - wp_db
     image: wordpress:latest
     ports: 
       - 80
     restart: always
     environment:
       WORDPRESS_DB_HOST: wp_db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
     networks:
     - npm_public
volumes:
    wp_db_data:
networks:
  npm_public:
    external: true
```