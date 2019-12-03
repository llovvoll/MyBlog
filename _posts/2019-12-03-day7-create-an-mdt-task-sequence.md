---
layout: single
title: "【Day 7】 建立MDT Task Sequence"
date: 2019-12-03
modified:
description: "Create an MDT Task Sequence"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2019/12/03/2019-12-03-012.png
---

基本的介紹都已經差不多了，今天我們就開始來建立第一個Task Sequence吧，可以開始佈署系統到電腦了，一開始我們一樣開啟Deployment Workbench對著Task Sequence按右鍵New Task Sequence，Task Sequence ID 必須是唯一值，建議可以設定較有識別性的ID，Task Sequence Name的話都可以

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-001.png)

在Select Template的話，通常只會使用到Standard Client Task Sequence及Standard Server Task Sequence，這邊我們選擇Client，如果有興趣可以玩玩看其它的選項

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-002.png)

接著Select OS的頁面就選擇要佈署哪個版本的作業系統並下一步，在Product Key的部份就看是否需要輸入，在我環境的做法都是選擇不輸入，因為機器本身已有買隨機的授權，另外我也有寫批次檔去做系統判斷切換成KMS啟動

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-003.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-004.png)

OS Settings也是依照需求更改，可以先預設即可，這邊我習慣用Database去做套用，Full Name及Organization對照在下圖中的紅框中的值

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-005.png)ㄑ
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-006.png)

Admin Password 可以先改自己所需的或是先不設定，這邊也是可以用Database套用，接著都按下一步就可以把Task Sequence建立起來了

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-007.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-008.png)

接著我們點進我們剛剛新建的Task Sequence，把一些不必要的Task先Disable避免出狀況，一般來說我會把下圖紅框處的選項全部停用，建議不熟悉的人直接停用即可，另外在Description的地方建議註記一下，這樣之後才知道為甚麼停用或有沒有更動，因為選項太多了，有時也會忘記哪些設定動過，像我只要更動過任何設定的話，就會註記一下日期

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-009.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-010.png)

都設定完成之後就Apply儲存，然後我們就可以在使用MDT的開機光碟或ISO進到佈署介面中來測試看看吧，輸入完帳號密碼就能看到Task Sequence選單了，這邊也能看到剛剛所建立的Task Sequence ID，接著就都下一步

> 如果成功驗證後發現畫面一樣是一片空白的話，請再次確認一下整個DeploymentShare的共享權限(包含子資料夾)是否權限正確

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-011.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-012.png)

這邊就全都下一步就會開始格式化硬碟及佈署作業系統囉

![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-013.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-014.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-015.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-016.png)
![]({{ site.url }}/assets/images/2019/12/03/2019-12-03-017.png)

那到這邊我們最基本的介紹就完成了，接下來就是會開始講如何優化佈署的流程跟技巧