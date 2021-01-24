---
layout: single
title: "輕鬆使用 Docker nginx 打包兩個 Vue 專案進行部署"
date: 2021-01-24
modified:
description: "輕鬆使用 Docker nginx 打包兩個 Vue 專案進行部署"
categories:
  - "Tech Article"
  - "Front-End"
tags:
  - Docker
  - Vue
---

前陣子準備部署一個專案，分別為前端使用者介面及管理員後台介面並切成兩個 Vue 專案，需求是希望統一打包成一個容器並使用資料夾的方式去區分，希望的網址呈現如下：

```
http://127.0.0.1/
http://127.0.0.1/admin/
```

前置作業需要先將 Vue 專案 Build 出來，再來開始撰寫 Dockerfile ， 當然也可以寫個小腳本來讓所有動作都自動化

# Dockerfile

```
FROM nginx

MAINTAINER RICK JIANG

COPY /admin/dist/ /usr/share/nginx/html/admin
COPY /frontend/dist/ /usr/share/nginx/html/
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
```

# Nginx Default.conf

```
server {
    gzip on;
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
    root /usr/share/nginx/html;
    index  index.html index.htm;
    try_files $uri $uri/ /index.html;
    }

    location /admin {
        alias /usr/share/nginx/html/admin;
        index index.html index.htm;
        try_files $uri $uri/ /admin/index.html;
    }

    location @rewrites {
        rewrite ^(.*)$ /index.html last;
    }
}
```

將 Dockerfile 及 Default.conf 跟 Vue 專案的資料夾放在同一個目錄下就可以執行 Docker build . ，簡簡單單就可以打包成容器並部署
