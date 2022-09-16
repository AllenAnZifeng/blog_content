# Blog Architecture
Date: 2022/09/13\
Description: The article demonstrates the software design and React implementation of the blog.\
Category: Blog\
Tag: React, Architecture
---
## React Components 

Utilized **client side routing**, the root will display Home page, the blog URL will display the corresponding article.
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
![architecture](resources/blog.png)

## Github Actions

The source code will be compiled and webpacked into the gh-pages branch on push.

[source code](https://github.com/AllenAnZifeng/blog_frontend/blob/master/.github/workflows/deployment.yml)

