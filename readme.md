# SWAN - Simple WeChat Alert Notifier

Based on [Laravel](https://laravel.com/) and [EasyWeChat](https://easywechat.org/).

## Requirements

+ Nginx
+ PHP 5.6+
+ MySQL 5.6+/MongoDB 3+

## Nginx

```
server {
    server_name swan.sample.com;
    listen 80;
    index index.php index.html index.htm;
    root /data/www/swan.sample.com/public/;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php($|/){
        fastcgi_pass          127.0.0.1:9000;
        fastcgi_index         index.php;
        include               fastcgi_params;
        set $path_info        "";
        set $real_script_name $fastcgi_script_name;

        if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
            set $real_script_name $1;
            set $path_info        $2;
        }

        fastcgi_param SCRIPT_FILENAME $document_root$real_script_name;
        fastcgi_param SCRIPT_NAME     $real_script_name;
        fastcgi_param PATH_INFO       $path_info;
        fastcgi_param PHP_VALUE       open_basedir=/data/www/swan.sample.com/:$document_root:/tmp/:/proc/:/dev/urandom;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
        expires      30d;
    }

    location ~ .*\.(js|css)?$ {
        expires      12h;
    }

    access_log  /data/wwwlog/swan.sample.com/access.log access;
    error_log  /data/wwwlog/swan.sample.com/error.log error;
}

```

## Project configuration

### Database

If you want to use MongoDB, you may config your .env with:

```
MONGO_DB_DSN=mongodb://username:password@host:port/database
MONGO_DB_DATABASE=swan
```

