---
layout: single
title: "【Day 4】 設定MDT Deployment Share基本參數及開機映像檔"
date: 2019-11-27
modified:
description: "Set up MDT Deployment Share Properties and boot image"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2019/11/27/2019-11-27-008.png
---

工作真的是有點忙，要每天都發文真的是有點難度，所以我還是堅持文章打好打滿就好XD，今天我們就先把MDT Deployment Share基本的設定給設定好，並且產生出我們第一個Windows PE boot image用來開機進入到MDT的佈署介面

<!-- Table of Contents -->
{% include toc icon="heart" title="Deployment Share Properties and Boot image" %}

## Setting - Deployment Share Folder Permissions
首先先進到安裝Microsoft Deployment Toolkit (MDT)的目錄，通常為C:\DeploymentShare，我們要來調整一下共用的權限，MDT在安裝時已經自動開啟了目錄共用，但權限的部分只有Administrator，這樣直接拿來佈署使用有點危險，所以通常會開唯讀權限給特定USER或GROUP，實務上來講會開給IT的USER GROUP唯讀即可，當開機進入到佈署介面時IT就使用各自的網域帳號登入即可

![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-001.png)
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-002.png)

## Setting - Deployment Share Properties
開啟Deployment Workbench，對著MDT Deployment Share按右鍵Properties，這頁面要注意的地方有兩點，Network (UNC) path及Platforms Supported，Network (UNC) path務必使用完整的網域名稱，例如\\\MDT-DEMO.ovvo.cc\DeploymentShare$，這個地方請務必注意，這邊也建議無論甚麼服務，如果能使用FQDN的方式就使用FQDN，這樣不用擔心哪天IP需要異動或是其它情形，只要有異動只要從DNS Server去做更動即可

Platforms Supported這邊依環境設定，我個人只有將x64打勾，Rules頁面的話我們先保持預設即可
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-003.png)
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-004.png)

Windows PE的頁面，預設畫面在x86，我們將Generate a Lite Touch bootable ISO image勾勾拿掉，我們不需要產生x86的boot image，x64的畫面也先預設即可，接著到Monitoring，把功能給Enable起來，Monitoring host也建議輸入完整的網域，Port號都保持預設，到這邊基本設定就完成
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-005.png)

## Builder - MDT boot image
對著MDT Deployment Share按右鍵Update Deployment Share並都按下一步就會開始建立boot image iso，第一次建立會比較久，這邊需要等一下
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-006.png)
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-007.png)

建立完成的iso檔，我們可以到C:\DeploymentShare\Boot中找到，接著我們就來測試看看iso及共用的權限是否正常

開機進入MDT後會看到這個介面，接著點選Run the Deployment Wizard to install a new Operating System會導至登入驗證的介面，這邊就輸入所設定的USER或USER GROUP內的USER Account，Domain記得要輸入，如果是本機的話輸入localhost即可
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-008.png)
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-009.png)

如果權限及設定沒有錯誤的話，你應該介面會是一片空白，這是正常的，因為我們並沒有設定任何Task Sequences，那到這邊我們就完成了基本設定及開機映像檔啦
![]({{ site.url }}/assets/images/2019/11/27/2019-11-27-010.png)