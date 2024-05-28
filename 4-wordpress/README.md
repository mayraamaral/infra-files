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
## 2. After configuring Wordpress' proxy host inside NPM (Nginx Proxy Manager), include the code below after the ```<?php>``` section and comments inside the ```wp-config.php``` file
```php
define('.COOKIE_DOMAIN.', 'blog.mayra.dev');
define('.SITECOOKIEPATH.', '.');

if(isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
        $list = explode(',',$_SERVER['HTTP_X_FORWARDED_FOR']);
        $_SERVER['REMOTE_ADDR'] = $list[0];
  }
define( 'WP_HOME', 'https://blog.mayra.dev' );
define( 'WP_SITEURL', 'https://blog.mayra.dev' );
$_SERVER['HTTP_HOST'] = 'blog.mayra.dev';
$_SERVER['REMOTE_ADDR'] = 'https://blog.mayra.dev';
$_SERVER[ 'SERVER_ADDR' ] = 'blog.mayra.dev';

$_SERVER['HTTPS']='on';

if ($_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https')
       $_SERVER['HTTPS']='on';
```
  
**Observation:** Replace ```blog.mayra.dev``` with your own domain.
## Credits
[Post in Wordpress forum](https://wordpress.org/support/topic/wp-behind-nginx-reverse-proxy/)
