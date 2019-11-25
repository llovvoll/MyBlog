---
layout: single
title: "【Day 3】 建立MDT Deployment Share及SQL Database"
date: 2019-11-25
modified:
description: "Create MDT Deployment Share and set up Database"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: https://i.imgur.com/nppTDGj.png
---

今天來到第三天了，不過好像應該說是第三篇才對...已經時隔三天後了XD，但是沒關係！發文的目的達到就好，我還是會努力完賽的!!那就開始吧，今天我們來介紹如何建立MDT Deplyment Share跟設定SQL Databse並與MDT做結合

<!-- Table of Contents -->
{% include toc icon="heart" title="MDT Deployment Share and Database" %}

## Create - MDT Deployment Share
首先我們先來把Deployment Share給建立起來，先開啟你的 "Deployment Workbench"，對著Deployment Shares右鍵並點選New Deployment Shares
![None]({{ "https://i.imgur.com/03kFpYB.png" | absolute_url }})

接下來Path、Share、Descriptive Name都保持預設值，只要下一步即可
![None]({{ "https://i.imgur.com/U98V1oT.png" | absolute_url }})
![None]({{ "https://i.imgur.com/fXnYLj0.png" | absolute_url }})
![None]({{ "https://i.imgur.com/u29DiWL.png" | absolute_url }})

在Optons這邊的選項全部不要勾選，這些我們都可以在之後的Task Sequences中進行設定
![None]({{ "https://i.imgur.com/bvovDCr.png" | absolute_url }})

這時你的Summary應該是像這樣子，沒有錯的話就可以按下一步將Deployment Share建立起來
![None]({{ "https://i.imgur.com/9MpPbll.png" | absolute_url }})
![None]({{ "https://i.imgur.com/i4590yp.png" | absolute_url }})

這樣子就完成了，接下來我們來安裝SQL Server Express
![None]({{ "https://i.imgur.com/8ZNaUxs.png" | absolute_url }})

## Install - SQL Server Express
> MDT 可以使用 SQL Server Express 或完整的 SQL Server，但是因為部署資料庫並不大 (即使在大型企業環境中)，我們建議在您的環境中使用免費的 SQL Server 2012 SP1 Express 資料庫。

因為MDT的資料庫很小，所以微軟也建議安裝Express即可，這邊我們選用最新的版本"SQL Server 2017 Express"

[https://www.microsoft.com/en-us/sql-server/sql-server-editions-express](https://www.microsoft.com/en-us/sql-server/sql-server-editions-express)

下載執行後，我們選擇Basic並按下一步開始安裝，接著等待完成即可
![None]({{ "https://i.imgur.com/JwTXVMi.png" | absolute_url }})

這邊我們點選Install SSMS(SQL Server Management Studio)，打開網頁後點擊Download SQL Server Management Studio (SSMS)執行安裝，安裝完成後重新啟動電腦
![None]({{ "https://i.imgur.com/hK0n1YY.png" | absolute_url }})
![None]({{ "https://i.imgur.com/8lubF6p.png" | absolute_url }})

## Setting - SQL Server Express & Firewall
SQL Server Express安裝完成後，我們必須先做一些基本設定才有辦法讓其它電腦Query

先開啟service.msc並找到SQL Server Browser並按右鍵Properties，將Startup type改為Automatic並Apply後再按右鍵Start
![None]({{ "https://i.imgur.com/XgBRjUp.png" | absolute_url }})

接著開啟Sql Server Configuration Manager，點選SQL Server Network Configuration > Protocols for SQLEXPRESS，把Named Pipes跟TCP/IP右鍵Enabled

接著對TCP/IP右鍵Properties > IP Addresses，拉到最下方將IPALL裡面的TCP Port改為1433並Apply
![None]({{ "https://i.imgur.com/axgTKcL.png" | absolute_url }})

Apply後，點選左邊選單中的SQL Server Service，將SQL Server(SQLEXPRESS)跟SQL Server Browser都按Restart，接著我們針對需要的Port在Firewall設定Allow，先用Administratr執行cmd並貼上以下三行指令

{% highlight bash %}
netsh advfirewall firewall add rule name="SQL Server Express" protocol=TCP dir=in localport=1433-1434 action=allow
netsh advfirewall firewall add rule name="MDT Monitoring" protocol=TCP dir=in localport=9800-9801 action=allow
netsh advfirewall firewall add rule name="MDT SMB" protocol=TCP dir=in localport=135-139 action=allow
{% endhighlight %}

![None]({{ "https://i.imgur.com/eoyJuRe.png" | absolute_url }})

## Setting - MDT Database
回到Deployment Workbench，點選MDT Deployment Share > Advanced Configuration 右鍵 New Database
![None]({{ "https://i.imgur.com/qjD0U4D.png" | absolute_url }})

SQL Server name 建議使用FQDN的方式，這邊範例我就先使用IP，Instance請輸入SQLEXPRESS、PORT留空、Network Library預設Named Pipes接著下一步
![None]({{ "https://i.imgur.com/SSyF5sv.png" | absolute_url }})

選擇Create a new database並輸入MDT並下一步
![None]({{ "https://i.imgur.com/5rIaD7S.png" | absolute_url }})

SQL Share 輸入 DeploymentShare$ 然後都按下一步
![None]({{ "https://i.imgur.com/SSyF5sv.png" | absolute_url }})

看到這邊就建立完成了，按Finsh即可
![None]({{ "https://i.imgur.com/Urxg5f4.png" | absolute_url }})

## Setting - SQL User & SQL Server
開啟Microsoft SQL Server Management Studio後直接按Connect，接著對你的HostName按右鍵Properties > Secunity把Server authentication改為SQL Server and Windows Autherntcation mode

接著點Security > Login 右鍵 New Login，Login name可以自行決定，記得請勾起SQL Server and Windows Autherntcation並把Enforce password policy這個三個取消勾選
![None]({{ "https://i.imgur.com/s1Xfikg.png" | absolute_url }})

然後到User Mapping，將MDT這個資料庫指派給MDTConnect並設定為唯讀，如下圖設定
![None]({{ "https://i.imgur.com/wYvLU22.png" | absolute_url }})

這樣我們就完成了建立Deployment Share跟SQL MDT Database，下一篇我們就開始學習如何建置MDT了

## References
[Use the MDT database to stage Windows 10 deployment information (Windows 10) - Microsoft Docs](https://docs.microsoft.com/en-us/windows/deployment/deploy-windows-mdt/use-the-mdt-database-to-stage-windows-10-deployment-information)