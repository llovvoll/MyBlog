---
layout: single
title: "【Day 5】 導入Windows作業系統ISO到MDT"
date: 2019-11-28
modified:
description: "Importing Operating System to MDT"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2019/11/28/2019-11-28-008.png
---

昨天已經完成MDT的開機映像檔也成功登入驗證到了MDT，今天我們就把第一個作業系統導入至MDT中，好讓之後建立Task Sequences時可以選用，接下來的幾篇我應該都會分開來講，這樣比較不會亂掉，閱讀起來也輕鬆一點

Windows ISO Download : [https://www.microsoft.com/en-us/software-download/windows10](https://www.microsoft.com/en-us/software-download/windows10) 

Windows Server ISO Download : [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server)

要導入系統到MDT非常的簡單，首先將要佈署的ISO檔下載到MDT的主機上，再將ISO檔掛載起來
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-001.png)
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-002.png)

接著開啟開啟Deployment Workbench，對著Operating System按右鍵New Folder，建議建立成階層式目錄，如下圖，這樣比較整齊也方便管理
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-003.png)

接著對你的目錄按右鍵 Import Operating System接著將Full set of source files勾選起來並下一步，接著選取你掛載ISO的虛擬光碟磁區並下一步
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-004.png)
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-005.png)

Destination directory name預設會抓取ISO的系統版本類型，這邊我會建議改成較好識別的，以免之後造成辨識困難
e.g. Windows 10 Home x64 > Win10_1909_English_x64
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-006.png)

更改完成都按下一步就會開始將作業系統的相關檔案導入至MDT中囉
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-007.png)
![]({{ site.url }}/assets/images/2019/11/28/2019-11-28-008.png)

這樣就成功導入作業系統ISO了，下一篇我們再來將電腦的Driver導入至MDT中