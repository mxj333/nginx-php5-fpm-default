# nginx-php5-fpm-default
nginx-php5-fpm-default

```
server {
    listen   80;
    root   /var/www/html;
    index  index.php index.html;

    access_log /var/log/nginx/access.log;
    error_log  /var/log/nginx/error.log;

    server_name localhost;
    sendfile off;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        try_files $uri /index.php=404;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME /var/www/html$fastcgi_script_name;
        include fastcgi_params;
    }
}

```