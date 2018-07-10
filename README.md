## **MariaDB**

MariaDB is a community-developed fork of the MySQL relational database management system, intended to remain free under the GNU GPL. Being a fork of a leading open source software system, it is notable for being led by the original developers of MySQL, who forked it due to concerns over its acquisition by Oracle. Contributors are required to share their copyright with the MariaDB Foundation.

## Docker Compose Example

   ```shell
   version: '2.1'
   
   services:
   
     api-server:
       container_name: api-server
       image: pam79/php-fpm
       working_dir: /usr/share/nginx/html
       volumes:
         - ./:/usr/share/nginx/html:z
       environment:
         - "DB_PORT=3306"
         - "DB_HOST=db-server"
   
     web-server:
       container_name: web-server
       image: pam79/nginx
       working_dir: /usr/share/nginx/html
       volumes_from:
         - api-server
       environment:
         - "VIRTUAL_HOST=api-dev.example.com"
       tty: true
       stdin_open: true
       networks:
         - default
   
     db-server:
       container_name: db-server
       image: pam79/mariadb
       volumes:
         - dbdata:/var/lib/mysql
       environment:
         - "MYSQL_DATABASE=apidb"
         - "MYSQL_USER=apiuser"
         - "MYSQL_PASSWORD=apisecret"
         - "MYSQL_ROOT_PASSWORD=mariadbrootpass"
       ports:
         - "33061:3306"
   
   volumes:
     dbdata:
   
   networks:
     default:
       external:
         name: proxy-tier
   ```




