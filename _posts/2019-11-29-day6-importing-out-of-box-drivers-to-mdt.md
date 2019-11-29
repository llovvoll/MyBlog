---
layout: single
title: "【Day 6】 導入電腦Windows驅動程式到MDT"
date: 2019-11-29
modified:
description: "Importing Out of Box Drivers to MDT"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2019/11/29/2019-11-29-006.png
---

今天是第六天，我們來把系統的驅動程式導入到MDT，好讓MDT在佈署系統時可以把正確的驅動程式交給佈署的電腦上，這邊要注意的事情是一樣是階層式目錄並且電腦型號別輸入錯誤，那就來看看怎麼做吧

## Checking - Computer Model
通常要建立新的電腦型號到MDT中時，我們會先確認電腦的型號是甚麼，並在MDT建立相關的目錄去放置驅動程式，這邊的正確性很重要，當MDT在佈署時會有腳本進行確認電腦型號並依照電腦型號去尋找MDT中所建立的階層目錄中的驅動程式，你可以使用cmd.exe並輸入Systeminfo找到電腦的型號

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-001.png)

↑ Dell OptiPlex 3020 所顯示的內容

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-002.png)

↑ Lenovo ThinkCentre M710s 所顯示的內容

注意到了嗎？在Lenovo聯想的機器使用Systeminfo所出現的內容是一串無法一眼辨識的型號，這大概是聯想自己本身內部的小型號，筆者有發現同個型號(M710s)，每批貨的型號顯示的也不同，所以這個狀況要特別注意，如果是聯想的機器你可以改使用下圖中的指令來查看正確的型號並使用這個型號去建立目錄才是正確的，之後我們會改寫腳本來針對Lenovo的電腦去抓取正確的型號

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-003.png)

{% highlight bash %}
@Echo Off
Wmic CSProduct Get Vendor
Wmic CSProduct Get Version
Pause
{% endhighlight %}

確認完正確的製造商及型號後，我們就依照階層式的方式去建立資料夾，這邊的目錄建立方式是為了配合之後在佈署時可以讓MDT自動判斷製造商及型號並到正確的目錄抓取驅動程式，目錄的結構請參考如下圖

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-004.png)

## Importing - Out of Box Drivers
建立完目錄後我們就可以來把驅動程式匯入到MDT中，通常電腦廠商都會有打包好的驅動程式佈署包，大致上關鍵字可以搜尋"SCCM Driver Package" or "Driver pack"，這邊分享一下我這邊主要使用的三大廠商下載連結

Lenovo : [https://support.lenovo.com/tw/en/solutions/ht074984](https://support.lenovo.com/tw/en/solutions/ht074984)

Dell   : [https://www.dell.com/support/article/tw/zh/twbsd1/sln312414/dell-command-deploy-driver-packs-for-enterprise-client-os-deployment?lang=en](https://www.dell.com/support/article/tw/zh/twbsd1/sln312414/dell-command-deploy-driver-packs-for-enterprise-client-os-deployment?lang=en)

HP     : [https://ftp.hp.com/pub/caps-softpaq/cmit/HP_Driverpack_Matrix_x64.html](https://ftp.hp.com/pub/caps-softpaq/cmit/HP_Driverpack_Matrix_x64.html)


這裡範例以HP EliteBook 830 G6為範例，下載最新的驅動程式包並解壓縮，並且注意讓它是一個獨立的目錄，因為導入驅動時MDT是連子資料都會一同導入，可以參考我的做法並也當成備份留存

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-005.png)

把驅動程式解壓縮出來後就可以回到Deployment Workbench，對你所建立的型號資料夾按右鍵Import Drivers

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-006.png)

這邊再次提醒，記得Driver的資料夾位置必須為獨立的，不要直接指向上一層，這樣MDT會把新舊版的Driver或其它東西一併導入，然後記得把Import drivers even if they are duplicates of an existing driver勾起，接著都按下一步即可將驅動程式導入至MDT中

![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-007.png)
![](https://ovvo.cc/assets/images/2019/11/29/2019-11-29-008.png)

如果有些Warmings是正常的，通常就是提醒不支援x86，但這不影響使用，按Finsh就完成將驅動程式導入至MDT了