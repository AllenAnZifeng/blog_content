# NYU网站项目笔记

Data: 2023/1/11\
Description: Web Dev\
Category: Web，Linux\
Tag: Web

## 背景介绍
帮老同学忙接手了一个做LGBTQ+调查问卷的项目，服务器没有root权限，只有LMAP环境，没有apt等包管理软件。之前跑路的码农使用了PHP，没有使用别的技术栈。

## 踩过的坑

- 没有Node，从source上wget安装后发现缺少system dependency，太复杂只能弃坑
- 没有Python3，从官网下载Python3 Linux Source Code在服务器上Build了一个Python3 [教程](https://pages.github.nceas.ucsb.edu/NCEAS/Computing/local_install_python_on_a_server.html)
- 有了Python3之后开了一个Flask，没有权限修改Apache配置文件，使用了[php-reverse-proxy](https://github.com/nbhr/php-reverse-proxy)
```editorconfig
RewriteRule ^flask/?(.*)$ /proxy.php?dst=http://127.0.0.1:8080/$1 [L] # 写在.htaccess
# ^ start of the string
# $ end of the string
# /? 0 or 1 /
# (.*) matching group, any character any number of repetition
# $1 first matching group
```
- 通过以上配置实现了访问 https://rookietherapist.hosting.nyu.edu/flask 跳转至服务器本地的Flask8080端口
- 服务器端的MySQL只允许本地连接,需要使用putty tunneling 转发端口连接
- 大坑:php的文件权限必须是644，一定要注意文件权限匹配，不然php不会生效
- 部署前端直接npm build到一个新branch上,到server上直接git clone已经built好的分支
- 后端Django只能git clone然后开venv,pip install,最后改写成systemd service

![architecture](https://raw.githubusercontent.com/AllenAnZifeng/blog_content/master/resources/blog8/nyu_project.png)


## Nginx Reverse Proxy 配置
- 由于linode.zifengallen.me域名配置过SSL证书，所以需要所有端口都添加SSL配置
- 一个SSL证书可以给这个Virtual Host的所有端口使用


```
# 这个配置文件在Linode服务器的 /etc/nginx/sites-available/linode.zifengallen.me
server {
listen [::]:8888 ssl ipv6only=on; # managed by Certbot
listen 8888 ssl; # managed by Certbot
ssl_certificate /etc/letsencrypt/live/linode.zifengallen.me/fullchain.pem; # managed by Certbot
ssl_certificate_key /etc/letsencrypt/live/linode.zifengallen.me/privkey.pem; # managed by Certbot
include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

server_name linode.zifengallen.me;
location / {
    proxy_pass http://127.0.0.1:8000;
 }
}
```

## Client Side Routing 问题
由于后端是API Django Server，并不认识前端的路由，所以需要在后端配置一个catch-all route,
让所有的路由都指向index.html，然后前端的React Router会自动处理路由。

- 由于没有权限, 继续写在.htaccess, 注意可以创建多个.htaccess可以放在不同的folder下
- Apache server 会改写访问的URL,如果不是index.html就会改成index.html返回给客户端
```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

## Django Systemd Service
- 文件位置 /etc/systemd/system/nyubackend.service
- 注意Python路径是venv里的Python
```editorconfig
[Unit]
Description=NYU Backend LGBT+

[Service]
User=root
Group=root
WorkingDirectory=/root/NYUBackend
ExecStart=/root/NYUBackend/venv/bin/python /root/NYUBackend/manage.py runserver
Restart=always

[Install]
WantedBy=multi-user.target
```
- reload systemd and start service
- enable to start automatically at boot time
- 查看日志并且follow
```bash
sudo systemctl daemon-reload
sudo systemctl start nyubackend
sudo systemctl enable nyubackend
sudo journalctl -u nyubackend -f 
```

