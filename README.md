# nginx-php5-fpm-default
在ubuntu 14.04中安装最新版的nginx和php5-fpm

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


If you do not have a domain name pointed at your server and you do not know your server's public IP address, you can find it by typing one of the following into your terminal:
```
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```

OR

```
curl http://icanhazip.com
```

###install mysql
```
sudo apt-get install mysql-server mysql-client -y 
```
First, we need to tell MySQL to generate the directory structure it needs to store its databases and information. We can do this by typing:
首先，我们需要告诉MySQL来产生，它需要存储它的数据库和信息的目录结构。我们可以通过键入做到这一点
```
sudo mysql_install_db
```

Next, you'll want to run a simple security script that will prompt you to modify some insecure defaults. Begin the script by typing:
接下来，你要运行一个简单的安全脚本，会提示你修改了一些不安全的默认值。首先输入脚本：

```
sudo mysql_secure_installation
```
