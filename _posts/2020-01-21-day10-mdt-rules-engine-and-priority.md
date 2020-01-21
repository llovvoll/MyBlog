---
layout: single
title: "【Day 10】 MDT Rules Engine and Priority"
date: 2020-01-21
modified:
description: "MDT Rules Engine and Priority"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2020/01/21/2020-01-21-mdt-running-sequence.png
---

經過前面九篇的文章，我們最基本的 MDT 佈署環境就建立完成了，這樣其實就已經非常好用了，但我們要繼續好要更好，現在開始就來講講比較深入的運用了，我們先來看看 MDT Rules 及當進入 MDT 時背景跑了哪些東西，驗證完帳號密碼之後又跑了哪些東西?

![]({{ site.url }}/assets/images/2020/01/21/2020-01-21-001.png)

最上面流程圖是我畫出來比較簡單清晰的流程，當進入到 MDT 時會先去讀取包在開機映像檔中的 Bootstrap.ini ，這邊是預載入的參數設定的地方，這邊會讀取應該要去哪台 MDT Server ，這邊也可以設置一些參數預設值，之後若要使用異地主機時也會在這邊操作

![]({{ site.url }}/assets/images/2020/01/21/2020-01-21-002.png)

接下來的步驟，當你輸入完帳號密碼後，會依據預載入中的 DeployRoot 所依據的 MDT Server 位置去做驗證並撈取 MDT Server 中的 CustomSettings.ini，這邊就可以加入自己所需要的參數預設值及工作優先順序，例如接下來我們會來講如何進入 MDT 時自動判斷機型與 DataBase 做查詢，並依據 DataBase 所設置的值來做佈署

基本上 MDT 的整體運作架構就是四個步驟，想要玩好 MDT 瞭解這四個流程非常重要，下篇我們就來看看如何修改 Rules(CustomSettings.ini) 吧