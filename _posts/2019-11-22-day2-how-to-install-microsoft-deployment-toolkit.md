---
layout: single
title: "【Day 2】 如何安裝Microsoft Deployment Toolkit (MDT)"
date: 2019-11-22
modified:
description: "How to install Microsoft Deployment Toolkit"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: https://i.imgur.com/iNjcYT4.png
---

今天是第二天，我們開始來把MDT整個系統架構環境架設起來，基本上完整的架構是Windows Deployment Services(WDS) + Microsoft Deployment Toolkit (MDT)，但因為WDS是需要Windows Server才有辦法使用，考量到系統授權的部分，我個人的做法是沒有使用到WDS，所以這系列並不會談到WDS的部份，有興趣的話可以看一下系統架構圖及參考其他人如何搭配使用

在我們的架構中並不會用到架構圖的右半邊的部份(WDS、PXE、DHCP)，基本上也不需要將環境搞得如此複雜，簡單的MDT+SQL Server Express並使用ADK做成Boot Image即可，如果要更方便一點的話，也可以自行架設一個PXE Server搭配使用也可以

<!-- Table of Contents -->
{% include toc icon="heart" title="How to install Microsoft Deployment Toolkit" %}

## Install - Microsoft Deployment Toolkit (MDT)
[https://www.microsoft.com/en-us/download/details.aspx?id=54259](https://www.microsoft.com/en-us/download/details.aspx?id=54259)

首先先將MDT安裝起來，打開網站並選擇64位元下載安裝，安裝的過程很簡單，只要下一步到底即可完成

![None]({{ "https://i.imgur.com/zwF8Q7F.png" | absolute_url }})
![None]({{ "https://i.imgur.com/UIJWpql.png" | absolute_url }})
![None]({{ "https://i.imgur.com/eElnsQS.png" | absolute_url }})
![None]({{ "https://i.imgur.com/a6689F7.png" | absolute_url }})

安裝完成若有看到新增這幾個程式就代表MDT安裝完成，其中的Deployment Workbench就是MDT的Console平台，之後主要的設置都會圍繞在這裡面

## Install - Deployment Kit (ADK)
[https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install](https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install)

[Download the Windows ADK for Windows 10, version 1903](https://go.microsoft.com/fwlink/?linkid=2086042)

> Windows 評定及部署套件 (Windows ADK) 擁有您針對大規模部署自訂 Windows 映像，以及測試系統、新增之元件及在系統上執行之應用程式的品質與效能時所需的工具。

基本上ADK的版本會跟著Windows的版本一同更新，要使用MDT來自定義映像檔就必須用到ADK，這兩者不可或缺，如果ADK的版本過舊就有可能發生無法佈署新版本系統的問題，但是建議如果使用上沒有問題的話，沒有必要追求新的版本，只求穩定即可，現在最新的版本是1903，微軟也有提到ADK不會發佈1909的版本，所以使用1903來佈署1909也是沒有問題的，像我目前使用的ADK還在1809的版本，要部署1909也是沒有問題的，目前也沒有遇到甚麼狀況

![None]({{ "https://i.imgur.com/gXkUK5Y.png" | absolute_url }})
![None]({{ "https://i.imgur.com/0FzQvHh.png" | absolute_url }})
![None]({{ "https://i.imgur.com/k3ACkw9.png" | absolute_url }})

開啟adksetup.exe後基本上也是一直下一步即可，但是第二個畫面會詢問是否傳送資料幫助改進ADK，這邊選擇No，接著按下一步就可以開始安裝，這邊會連網與微軟下載安裝的所需檔案，會需要一點時間

![None]({{ "https://i.imgur.com/nS6oHdZ.png" | absolute_url }})

看到這個畫面就代表ADK安裝完成，接著我們來安裝ADK的Windows PE附加元件，這個元件主要是建立Bootable ISO image，就是可以拿來做成開機光碟或是USB來進到MDT的佈署畫面，所以這個元件也是必須安裝的

[Download the Windows PE add-on for the ADK](https://go.microsoft.com/fwlink/?linkid=2087112)

![None]({{ "https://i.imgur.com/f2msOWt.png" | absolute_url }})
![None]({{ "https://i.imgur.com/iColRZ5.png" | absolute_url }})

這邊也是一直下一步即可，下載的大小約5GB左右，這邊需要等待一段時間～

![None]({{ "https://i.imgur.com/xpT3ko5.png" | absolute_url }})

這樣MDT+ADK必要的元件就安裝完畢了，明天我們再來新建一個Deployment Shares並安裝SQL Server Express，並在MDT中將Database掛載起來

## References
[MDT with WDS Integration Overview - Tech Thoughts](https://techthoughts.info/mdt-with-wds-integration-overview/)

[Download Microsoft Deployment Toolkit (MDT) from Official Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=54259)

[Download and install the Windows ADK - Microsoft Docs](https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install)