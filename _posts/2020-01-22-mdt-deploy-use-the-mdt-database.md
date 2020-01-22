---
layout: single
title: "【Day 11】 MDT Deploy Use the MDT Database"
date: 2020-01-22
modified:
description: "MDT Deploy Use the MDT Database"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2020/01/22/2020-01-22-001.png
---

之前我們的佈署方式都是在 MDT 選單中手動來選擇 Task Sequences 及 Applications ，但使用起來還是不夠方便，今天我們就來看看如何在 MDT 資料庫中新建特定的機器型號供 MDT 佈署時自動與資料庫查詢匹配並套上我們在資料庫上所設定好的 Rules

起手式一樣先開啟 Deployment Workbench 點開 Advanced Configuration > Database 右鍵 > Configure Database Rules，如果還沒有設定 Database 的話可以看一下 [Day 3](https://ovvo.cc/day3-create-deployment-share-and-set-up-database/) 的文章

![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-001.png)

### 這邊先解釋一下圖中的四個選項意思及主要功能

* Computer Options   => 可以在資料庫中設定指定的 Serial Number 或 MAC Address 來做佈署，當進入到 MDT 中會自動與資料庫查詢，如果前面的值有在資料庫匹配到的話，就會依據參數設定來佈署

* Location Options   => 可以在資料庫中設定Default GateWays，未來如果要做分散式佈署的時候會使用到這個，當 MDT 要找哪台 MDT Server 的時候會先到資料庫查詢哪個 GateWays 要去哪台比較近的 Server

* Make/Model Options => 可以在資料庫新增特定的機器型號，供 MDT 佈署時查詢特定的型號該執行哪些動作

* Role Options       => 可以在資料庫新增規則，例如 HP 的佈署規則、 Lenovo 的佈署規則、 HQ 的佈署規則， OBU 的佈署規則，並套用到以上三個選項中

這邊我們就把這四個選項都開啟起來，另外我習慣將 Query for SMS packages to be installed on this computer 及 Query for administrator to be assigned to this computer 取消勾選，因為其實用不太到，接下來就都下一步即可

![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-002.png)
![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-003.png)
![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-004.png)

完成後我們可以看一下 MDT Deployment Share 內的 Rules，剛剛的動作 MDT 就自動幫我們新建我們所選的選項到 Rules中了，然後這邊可以注意一下 Settings 中的 Priority ，越前面則代表優先順序較高，例如 CSettings 在 MMSettings 前面，所以到 MDT 佈署時同時查詢到符合的 MAC Address 及 機器型號 ，這時 MAC Address 的資料庫參數順序最高，佈署時會優先套用這邊的參數， MMSettings的值則不會被套到，反之如果前面的都沒有查詢到相符合的條件時則往下個繼續查詢

接著既然我們都已經要開始使用資料庫來設定佈署的話，這邊我們就加入一些略過的參數，這樣進入 MDT 時就不會像之前一樣跳出許多視窗需要點擊或選擇的，我們加入以下選項

```
;Skip
SkipAdminPassword=YES
SkipApplications=YES
SkipBDDWelcome=YES
SkipBitLocker=YES
SkipCapture=YES
SkipComputerBackup=YES
SkipComputerName=YES
SkipDomainMembership=YES
SkipFinalSummary=YES
SkipLocaleSelection=YES
SkipPackageDisplay=YES
SkipProductKey=YES
SkipSummary=YES
SkipTaskSequence=YES
SkipTimeZone=YES
SkipUserData=YES
SkipWizard=YES
```

接著還有一個很重要的地方，我們在每一個 Query 選項中加入資料庫的連線帳號及密碼資訊，否則在 MDT 中時會無法正常與資料庫連線，然後建議 SQLServer 這個值還是改為FQDN的方式

![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-005.png)
![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-006.png)

接著我們就來測試一下看看功能正常不正常，我們先用 MAC Address 的方式來測試，首先先看一下要佈署的電腦 MAC Address 是甚麼

![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-007.png)
![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-008.png)

回到 Deployment Workbench > Advanced Configuration > Database > Computers 右鍵 New ，填上 Description 及 MAC Address ，接著點開 Details 我們來設定需要執行的 Task Sequences ，設定完之後就可以儲存，接著開啟 MDT 來測試看看，因為剛剛我們已經把很多選項給 Skip 掉了，所以如果資料庫查詢有異常或是沒有找到匹配的時候應該會是一片空白，反之如果正常就會直接開始佈署了

![]({{ site.url }}/assets/images/2020/01/22/2020-01-22-009.gif)
