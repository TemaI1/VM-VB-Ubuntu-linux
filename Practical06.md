# Запуск веб-приложения из контейнеров

* Установить в виртуальную машину или VDS Docker, настроить набор контейнеров через docker compose по инструкции по ссылке: https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-co mpose-ru. Часть с настройкой certbot и HTTPS опустить, если у вас нет настоящего домена и белого IP.

>sudo apt install docker.io установил docker

>sudo apt install docker-compose установил docker-compose
https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose-ru начал работу по инструкции по ссылке

>mkdir wordpress создал директорию проекта

>mkdir nginx-conf создал директорию для файла конфигурации

>nano nginx.conf создал и открыл файл

>добавил в файл следующий код(серверный блок с директивами для имени нашего сервера и корневой директории документов, а также блок расположения для направления запросов сертификатов от клиента Certbot, обработки PHP и запросов статичных активов):

    server {
        listen 80;
        listen [::]:80;

        server_name example.com www.example.com;

        index index.php index.html index.htm;

        root /var/www/html;

        location ~ /.well-known/acme-challenge {
                allow all;
                root /var/www/html;
        }

        location / {
                try_files $uri $uri/ /index.php$is_args$args;
        }

        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass wordpress:9000;
                fastcgi_index index.php;
                include fastcgi_params;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_param PATH_INFO $fastcgi_path_info;
        }

        location ~ /\.ht {
                deny all;
        }

        location = /favicon.ico {
                log_not_found off; access_log off;
        }
        location = /robots.txt {
                log_not_found off; access_log off; allow all;
        }
        location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
                expires max;
                log_not_found off;
        }
}

>nano .env создал и открыл файл

>добавил в файл следующие имена и значения переменных:

    MYSQL_ROOT_PASSWORD=*****
    MYSQL_USER=mayDB
    MYSQL_PASSWORD=*****

>nano docker-compose.yml создал и открыл файл

>добавил следующий код для определения версии файла Compose и базы данных db:

    version: '3'

    services:
    db:
        image: mysql:8.0
        container_name: db
        restart: unless-stopped
        env_file: .env
        environment:
            - MYSQL_DATABASE=wordpress
        volumes:
            - dbdata:/var/lib/mysql
        command: '--default-authentication-plugin=mysql_native_password'
        networks:
            - app-network

>добавил определение для службы приложения wordpress:

    wordpress:
        depends_on:
            - db
        image: wordpress:5.1.1-fpm-alpine
        container_name: wordpress
        restart: unless-stopped
        env_file: .env
        environment:
            - WORDPRESS_DB_HOST=db:3306
            - WORDPRESS_DB_USER=$MYSQL_USER
            - WORDPRESS_DB_PASSWORD=$MYSQL_PASSWORD
            - WORDPRESS_DB_NAME=wordpress
        volumes:
            - wordpress:/var/www/html
        networks:
            - app-network

>добавил следующее определение для службы Nginx webserver:

    webserver:
        depends_on:
            - wordpress
        image: nginx:1.15.12-alpine
        container_name: webserver
        restart: unless-stopped
        ports:
            - "80:80"
        volumes:
            - wordpress:/var/www/html
            - ./nginx-conf:/etc/nginx/conf.d
            - certbot-etc:/etc/letsencrypt
        networks:
            - app-network

>добавил определения сети и тома:

    volumes:
        certbot-etc:
        wordpress:
        dbdata:

    networks:
        app-network:
            driver: bridge 

>sudo docker-compose up -d создал контейнеры и запуск в фоновом режиме

>docker-compose ps проверил статус служб 
