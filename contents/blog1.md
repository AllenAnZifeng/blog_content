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

![architecture](https://github.com/AllenAnZifeng/blog_content/blob/master/resources/blog.png?raw=true)

##  Github Actions

The source code will be compiled and webpacked into the gh-pages branch on push.

[source code](https://github.com/AllenAnZifeng/blog_frontend/blob/master/.github/workflows/deployment.yml)

