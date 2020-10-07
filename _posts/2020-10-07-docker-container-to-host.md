---
layout: single
title: "Docker 容器連接到宿主主機的服務"
date: 2020-10-07
modified:
description: "Docker 容器連接到宿主主機的服務"
categories:
  - "Tech Article"
tags:
  - Docker
---

前幾天在打包一個伺服器端的服務時，想要連接到宿主主機的資料庫服務時發現連接不上，雖然 Ping 的到宿主的 IP 位址，但是都無法成功連接

Google 了一下發現可以使用 Docker 內所附的特殊 DNS 名稱來解析到主機的內部 IP 即可成功連接

```
macOS & Windows
Docker v18.03版本以上 (2018年3月21日起)
host.docker.internal

Docker for macOS v17.12 ~ 18.02
docker.for.mac.host.internal

Docker for macOS v17.06 ~ 17.11
docker.for.mac.localhost
```

[Networking features in Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/networking/)