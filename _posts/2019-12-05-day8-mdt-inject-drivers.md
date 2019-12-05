---
layout: single
title: "【Day 8】 MDT Inject Drivers"
date: 2019-12-05
modified:
description: "MDT Inject Drivers"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2019/12/05/2019-12-05-005.png
---

上一篇我們已經建立了第一個Task Sequence，但是系統非常的乾淨，沒有安裝應用程式或是驅動程式，今天我們就來教教如何讓Task Sequence在佈署時偵測佈署電腦的型號自動與MDT的Drivers Box進行Mapping Inject Drivers

一樣先開啟Deployment Workbench並開啟你的Task Sequence，並在Task Sequence中建立一個變數，點Add > General > Set Task Sequence Variable，Name我們可以打Set DriverGroup比較好識別，變數的名稱就輸入"DriverGroup001"，這個變數是甚麼意思？當你有設定這個變數時，MDT在運作時會自動到這個位置查找Drivers，在MDT有許多變數可以利用，甚至可以自己建立變數寫程式去做判斷運用，那個就比較複雜就先不談，如果有興趣知道更多的內建變數可以看看下面微軟的頁面

[https://docs.microsoft.com/en-us/configmgr/mdt/toolkit-reference](https://docs.microsoft.com/en-us/configmgr/mdt/toolkit-reference)

"DriverGroup001"的Value就看你當初在Out-of-Box Drivers的目錄結構去設定，像我的結構就是輸入"Windows10\x64\%Make%\%Model%"，在MDT運行時會自動去獲取一些基本的值，所以這邊我們就套入已經取得到的變數到路徑中，比如說我的電腦所取得的製造商是HP，型號是HP EliteBook 830 G6，這時"DriverGroup001"的Value就會套入成"Windows10\x64\HP\HP EliteBook 830 G6"，所以記得大小寫及有沒有空格都要注意，否則MDT會找不到位置的

![]({{ site.url }}/assets/images/2019/12/05/2019-12-05-001.png)
![]({{ site.url }}/assets/images/2019/12/05/2019-12-05-002.png)

另外記得把Set DriverGroup的順序改為在"Inject Drivers"上面，並且把"Inject Drivers"的Choose a selection profile改為Nothing，並把Install all drivers from selection profile勾起來

![]({{ site.url }}/assets/images/2019/12/05/2019-12-05-003.png)

這時候就可以開機進到MDT測試看看了，也告訴大家如何查看Log，MDT是很方便的工具但容錯率非常的低，如果有Error的發生，後續的動作都會卡住無法繼續，所以學會看Log Debug很重要，在MDT佈署介面的時候可以按F8開啟Command，並CD到X:\MININT\SMSOSD\OSDLOGS，主要可以看BDD.log，那我們就來看看Set DriverGroup有沒有成功把"DriverGroup001"變數寫入

![]({{ site.url }}/assets/images/2019/12/05/2019-12-05-004.png)

紅框處可以看到MDT可以抓取成功製造廠商及型號，製造廠商是innotek Gmbh，型號是VirtualBox，接下來DriverGroup001也抓取成功了，這樣子就完成自動判斷製造廠商及型號去撈取Drivers了

![]({{ site.url }}/assets/images/2019/12/05/2019-12-05-005.png)
![]({{ site.url }}/assets/images/2019/12/05/2019-12-05-006.png)