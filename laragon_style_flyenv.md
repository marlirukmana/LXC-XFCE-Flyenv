create file:
/home/developer/.config/FlyEnv/server/vhost/nginx/gateway.conf

put that:

```
server {
    listen 80 default_server;
    server_name _;

    root /home/developer/Desktop/Projects;

    index index.php index.html index.htm;

    #
    # Default routing ke index.php
    #
    location / {
        try_files $uri $uri/ /index.php?$query_string;

        location ~ [^/]\.php(/|$) {
            fastcgi_pass   unix:/tmp/phpwebstudy-php-cgi-83.sock;
            fastcgi_index  index.php;
            include        fastcgi.conf;
            include        pathinfo.conf;
            fastcgi_split_path_info ^(.+?\.php)(/.*)$;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PATH_INFO        $fastcgi_path_info;
        }
    }

    # untuk php biasa application
    #----------------------------------------------------------------------------------------------------------------------------
    #
    # Informasi manual folder:
    # - /home/developer/Desktop/Projects/ adalah root direktori proyek
    # - Setiap folder di dalam Projects akan otomatis diakses via /<nama_folder>/
    # - Contoh: /home/developer/Desktop/Projects/myapp -> http://localhost/myapp/
    # - Folder dengan konfigurasi khusus (seperti phpmyadmin, laravel) perlu location ^~ eksplisit
    #
    # location ^~ /portal {
    #     index index.php;
    #     try_files $uri $uri/ /portal/index.php?$query_string;

    #     location ~ [^/]\.php(/|$) {
    #         fastcgi_pass   unix:/tmp/phpwebstudy-php-cgi-83.sock;
    #         fastcgi_index  index.php;
    #         include        fastcgi.conf;
    #         include        pathinfo.conf;
    #         fastcgi_split_path_info ^(.+?\.php)(/.*)$;
    #         fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    #         fastcgi_param  PATH_INFO        $fastcgi_path_info;
    #     }
    # }

    #
    # Auto-detect folder lokal (serve dengan PHP)
    # Otomatis deteksi semua folder di /home/developer/Desktop/Projects
    # Folder yang perlu proxy (phpmyadmin, laravel) pakai ^~ eksplisit di atas
    #
    location ~ ^/(?<app>[^/.]+)(?<path>/.*)?$ {
        try_files $uri $uri/ /$app/index.php?$query_string;

        #
        # PHP -> PHP-FPM (mengikuti pola enable-php-83.conf FlyEnv)
        #
        location ~ [^/]\.php(/|$) {
            fastcgi_pass   unix:/tmp/phpwebstudy-php-cgi-83.sock;
            fastcgi_index  index.php;
            include        fastcgi.conf;
            include        pathinfo.conf;
            fastcgi_split_path_info ^(.+?\.php)(/.*)$;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PATH_INFO        $fastcgi_path_info;
        }
    }
    #----------------------------------------------------------------------------------------------------------------------------

    # untuk proxy Application 
    #----------------------------------------------------------------------------------------------------------------------------
    #
    # phpMyAdmin -> phpmyadmin.test
    #
    location = /phpmyadmin {
        return 301 /phpmyadmin/;
    }

    location ^~ /phpmyadmin {
        client_max_body_size 200M;
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
    location ^~ /laravel {
        client_max_body_size 200M;
        proxy_set_header Host laravel.test;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://127.0.0.1/;
        proxy_redirect / /laravel/;
        proxy_redirect http://127.0.0.1/ /laravel/;
    }

    #
    # Proxy locations (otomatis)
    # Otomatis deteksi semua folder di /home/developer/Desktop/Projects
    # Host otomatis = <nama_folder>.test
    # proxy_redirect otomatis = / -> /<nama_folder>/
    #
    # location ~ ^/(?<app>[^/.]+)(?<path>/.*)?$ {
    #     if ($path = "") {
    #         return 301 /$app/;
    #     }
    #     client_max_body_size 200M;
    #     proxy_set_header Host $app.test;
    #     proxy_set_header X-Real-IP $remote_addr;
    #     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #     proxy_set_header X-Forwarded-Proto $scheme;
    #     proxy_pass http://127.0.0.1$path$is_args$args;
    #     proxy_redirect / /$app/;
    #     proxy_redirect http://127.0.0.1/ /$app/;
    # }
    
    #----------------------------------------------------------------------------------------------------------------------------

}


```
