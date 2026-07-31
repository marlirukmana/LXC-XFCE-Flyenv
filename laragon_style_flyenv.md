create file:
/home/developer/.config/FlyEnv/server/vhost/nginx/gateway.conf

put that:

```
server {
    listen 80 default_server;
    server_name _;

    root /home/developer/Desktop/Projects;
    index index.php index.html;

    #
    # phpMyAdmin -> phpmyadmin.test
    #
    location = /phpmyadmin {
        return 301 /phpmyadmin/;
    }

    location /phpmyadmin {
        proxy_set_header Host phpmyadmin.test;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://127.0.0.1/;
        proxy_redirect / /phpmyadmin/;
        proxy_redirect http://127.0.0.1/ /phpmyadmin/;
    }

    #
    # laravel
    #
    location /laravel {
        proxy_set_header Host laravel.test;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://127.0.0.1/;
        proxy_redirect / /laravel/;
        proxy_redirect http://127.0.0.1/ /laravel/;
    }

    #
    # Manual Laravel
    #
    location /emcare {
        alias /home/developer/projects/emcare/public/;
        index index.php;
        try_files $uri $uri/ /emcare/index.php?$query_string;
    }

}

```

create file:
/home/developer/.config/FlyEnv/server/vhost/nginx/gateway-auto.conf

```
server {
    listen 80 default_server;
    server_name _;

    root /home/developer/Desktop/Projects;
    index index.php index.html;

    #
    # Proxy locations (otomatis)
    # Otomatis deteksi semua folder di /home/developer/Desktop/Projects
    # Host otomatis = <nama_folder>.test
    # proxy_redirect otomatis = / -> /<nama_folder>/
    #
    location ~ ^/(?<app>[^/.]+)(?<path>/.*)?$ {
        if ($path = "") {
            return 301 /$app/;
        }
        proxy_set_header Host $app.test;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://127.0.0.1$path$is_args$args;
        proxy_redirect / /$app/;
        proxy_redirect http://127.0.0.1/ /$app/;
    }
}

```
