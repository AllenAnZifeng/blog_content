# Blog Architecture

Date: 2022/09/13\
Description: The article demonstrates the software design and React implementation of the blog\
Category: Blog\
Tag: React, Architecture

## React Components

Utilized **client side routing**\
Custom 404.html redirects URL to https://blog.zifengallen.me/?/blog/blog1.md \
The browser would ignore everything after ? and visit index.html which contains the redirection\
[Reference](https://github.com/rafgraph/spa-github-pages)

更新：
在React Router中使用Hash Router后解决了这个问题，缺点是URL中出现#

```
App
│
└─── "/" 
│    └─── HomePage
│             └─── Header
│             └─── Body
│             │      └─── Card
│             └─── Footer
│   
└─── "/blog/:filename"
    └─── ArticlePage
              └─── Header
              └─── Article
              └─── Footer  
```

## Architecture

![architecture](https://raw.githubusercontent.com/AllenAnZifeng/blog_content/master/resources/blog1/network_topology_edited.jpg)

##  Github Actions

The source code will be compiled and webpacked into the gh-pages branch on push.

[source code](https://github.com/AllenAnZifeng/blog_frontend/blob/master/.github/workflows/deployment.yml)

## 踩过的坑

- Ubuntu上root和普通用户看到的docker不一样
- Nginx同domain不同port可以使用相同SSL证书
- Nginx配置proxy_pass http://172.16.1.1:8000; 注意8000后面没有/，这样才能把整个url的完整路径转发过去
- Wireguard构建子网IP，端口转发可以用Zerotier加上Nginx逆向代理替换
- Docker在build过程中开起Node服务会导致超时，因为Node在前台运行，所有setup脚本都应该在Docker起来后跑，注意RUN和CMD区别[Ref](https://stackoverflow.com/questions/37461868/difference-between-run-and-cmd-in-a-dockerfile#:~:text=RUN%20is%20an%20image%20build,you%20launch%20the%20built%20image.)
- 通过Docker attach来看获取容器中的输入输出，将自己的terminal attach到容器上
- GitHub Pages custom Domain是在gh-pages branch上commit CNAME,每次commit后CNAME消失是因为CICD的时候没把CNAME复制过去


