create file:
/home/developer/.config/FlyEnv/server/vhost/nginx/gateway.conf

put that:

server {

    listen 80 default_server;

    server_name _;



    root /home/developer/Desktop;

    index index.php index.html;



    include enable-php-83.conf;



    #

    # phpMyAdmin -> phpmyadmin.test

    #

    location = /phpmyadmin {

        return 301 /phpmyadmin/;

    }



    location /phpmyadmin/ {



        proxy_set_header Host phpmyadmin.test;

        proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_set_header X-Forwarded-Proto $scheme;



        proxy_pass http://127.0.0.1/;

    }



    #

    # SDLC

    #

    location /sdlc {

        alias /home/developer/Desktop/SDLC/;

        index index.php index.html;

        try_files $uri $uri/ /sdlc/index.php?$query_string;

    }



    #

    # Emcare

    #

    location /emcare {

        alias /home/developer/projects/emcare/public/;

        index index.php;

        try_files $uri $uri/ /emcare/index.php?$query_string;

    }



    #

    # Inventory

    #

    location /inventory {

        alias /home/developer/projects/inventory/public/;

        index index.php;

        try_files $uri $uri/ /inventory/index.php?$query_string;

    }



    #

    # MPS

    #

    location /mps {

        alias /home/developer/projects/mps/public/;

        index index.php;

        try_files $uri $uri/ /mps/index.php?$query_string;

    }

}
