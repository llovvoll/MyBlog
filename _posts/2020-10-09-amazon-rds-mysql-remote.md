---
layout: single
title: "Amazon RDS MySQL 開啟遠端連接"
date: 2020-10-09
modified:
description: "Amazon RDS MySQL 開啟遠端連接"
categories:
  - "Tech Article"
tags:
  - AWS
  - RDS
header:
  image: /assets/images/2020/10/09/2020-10-09-001.png
---

首先進到自己的資料庫主機資訊頁中，選擇所屬的 VPC security groups，並進到設定頁面，在 Inbound rules 的頁面點擊 Edit inbound rules
在裡面新增規則選擇 MYSQL/Aurora 並將 Source 指向到 0.0.0.0 即可，若有其它安全性考量可以再做個別調整即可


![]({{ site.url }}/assets/images/2020/10/09/2020-10-09-001.png)

![]({{ site.url }}/assets/images/2020/10/09/2020-10-09-002.png)