---
title: "HuGo基本使用和部署"
date: 2021-01-23T22:39:54+08:00
summary: "HuGo基本使用和部署"
draft: false
toc: true
tags: ['docker','hugo']
author: "bitmyth"

---





## HuGo部署简介

HuGo是一个静态网站生成工具，支持多种主题。主题可插拔，可覆盖，支持极高的可定制性和灵活性。

创建新文章，命令如下：

```bash
hugo new posts/demo.md
```

生成静态网页，命令如下：

```bash
hugo
```

命令输出如下：

```Start building sites … 
                   | ZH  
-------------------+-----
  Pages            | 27  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     |  7  
  Processed images |  0  
  Aliases          |  2  
  Sitemaps         |  1  
  Cleaned          |  0  
```

## WEB 服务

开发环境下启动WEB服务，修改内容后支持实时可见，命令如下：

```bash
hugo server
```

命令输出类似如下：

```Start building sites … 
                   | ZH  
-------------------+-----
  Pages            | 27  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     |  7  
  Processed images |  0  
  Aliases          |  2  
  Sitemaps         |  1  
  Cleaned          |  0  

Built in 111 ms
Watching for changes in /Users/gsh/projects/kaifalu.com/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /Users/gsh/projects/kaifalu.com/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at //localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

生成静态资源后

会生成public文件夹，其中包含网站的所有静态资源。

## 部署

可以使用nginx容器部署网站

```bash
docker run -p80:80  -v $(pwd)/public:/usr/share/nginx/html:ro -d nginx
```

或者docker-compose，编写docker-compose.yaml内容如下：

```yaml
version: "3.3"

services:
  web:
    image: nginx:alpine
    volumes:
    - ./public:/usr/share/nginx/html
    ports:
      - "80:80"
```

这里使用体积较小的nginx:alpine镜像

使用docker-compose运行容器，命令如下：

```bash
docker-compose up -d
```