---
layout: single
title: "【Day 9】 MDT Deploy Applications"
date: 2020-01-20
modified:
description: "MDT Deploy Applications"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2020/01/20/2020-01-20-007.png
---

今天我們來看看如何在 MDT 中加入程式，讓 MDT 在佈署時可以選擇所需要的程式一同安裝，內文也會有幾種較常見的程式安裝方式給大家參考

一樣先開啟 Deployment Workbench，對著 Applications 右鍵新增資料夾來做分類，這邊以 Google Chrome Enterprise 為例

![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-001.png)

接著就先把 Chrome Enterprise 的安裝檔下載回來到 MDT 裡，到這裡[https://cloud.google.com/chrome-enterprise/browser/download/](https://cloud.google.com/chrome-enterprise/browser/download/)

下載回來之後解壓縮，只取裡面的 GoogleChromeStandaloneEnterprise64.msi 就可以了，然後另外新增一個資料夾並放進去，避免 MDT 在匯入的時候也一同匯入其它有的沒有的

![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-002.png)
![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-003.png)

回到 Deployment Workbench 對著剛剛建立的資料夾，Chrome Enterprise 按右鍵 New Application，勾選 Application with source files
這邊的欄位只有 Application Name 是必填，但我的習慣都會填寫

![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-004.png)

Source directory 就指向安裝檔的資料夾位置，記得將 Move the files to the deployment share instead of copying them 勾起並下一步即可

![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-005.png)

到了 Command Details 這邊就要打上所配合的安裝指令，以 Google Chrome Enterprise 為例，這邊就輸入以下的指令

```
msiexec /q /i "GoogleChromeStandaloneEnterprise64.msi"
```

輸入完成後下一步即可將程式匯入到 MDT 中，接著就可以進到 MDT 佈署介面試試看有沒有成功

![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-006.png)

如果有出現在 MDT 佈署介面中的 Applications 選單就成功囉，文章底下我附上 7-Zip 及 Acrobat Reader DC 的程式安裝指令，其它的大家可以 Google 上搜尋相關的關鍵字找到

![]({{ site.url }}/assets/images/2020/01/20/2020-01-20-007.png)

```
<!-- Chrome Enterprise  -->
msiexec /q /i "GoogleChromeStandaloneEnterprise64.msi"

<!-- 7-Zip -->
7z1900-x64.exe /S

<!-- Acrobat Reader DC -->
AcroRdrDC1900820071_zh_TW.exe /msi EULA_ACCEPT=YES  REMOVE_PREVIOUS=YES /qn
```