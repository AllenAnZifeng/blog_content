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
- 大坑:php的文件权限必须是644，一定要注意文件权限匹配，不然php不会生效
