# nginx-php5-fpm-default
nginx-php5-fpm-default

更新系统及添加非root用户
```
sudo apt-get update
sudo apt-get upgrade
sudo adduser test
sudo usermod -a -G sudo test
```

以test登录
运行：
```
sudo apt-get install software-properties-common python-software-properties

sudo add-apt-repository ppa:ondrej/php5-5.6
sudo apt-get update
sudo apt-get install -y php5-fpm php5-cli php5-curl php5-gd php5-json php5-mcrypt php5-mysqlnd
```

php-fpm 主配置文件
/etc/php5/fpm/php-fpm.conf

修改为以下内容：

指定时间内，如果失败的php-fpm 子进程数超过这个值，php-fpm主进程就优雅重启。
```
emergency_restart_threshold = 10
```

设定采用的时间跨度
```
emergency_restart_interval = 1m
```

配置进程池

让PHP-FPM加载/etc/php5/fpm/pool.d/目录中的各个进程池定义文件

include=/etc/php5/fpm/pool.d/*.conf

www.conf为PHP-FPM进程池默认配置文件。


重启PHP-FPM进程
```
sudo service php5-fpm restart
```

安装nginx
```
sudo add-apt-repository ppa:nginx/stable;
sudo apt-get update
sudo apt-get install iy nginx
```

cd /etc/nginx/sites-available/

编辑 default.conf

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