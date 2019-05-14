# docker-lamp
基于Docker的LAMP开发环境。

## 运行

创建目录 

```
mkdir -p ~/www/nginx
```

在设置 ~/www/nginx 目录添加 nginx 配置文件内容如下:

```
server {
       listen 80;
       listen [::]:80;

       server_name hdcms.test;

       root /var/www/html/hdcms;
       index index.php;

       location / {
               try_files $uri $uri/ =404;
       }
       location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.2-fpm.sock;
        }
}
```

修改 `/etc/hosts`添加 hdcms.test 本地域名的解析记录

```
...
127.0.0.1       hdcms.test
...
```

运行容器

```
docker run -tid -p 80:80 -p 3306:3306 -v ~/www:/var/www/html -v ~/www/nginx:/etc/nginx/conf.d 3b23ea1068a8
```

宿主使用 `localhost:80` (如果宿主机80端口已经被占用请更换射) 访问nginx