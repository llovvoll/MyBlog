---
layout: single
title: "How to Solve Downloading Language Dictionary"
date: 2018-10-31
modified:
description:
categories:
    - "Tech Article"
tags:
    - Windows
header:
---

前陣子同事的電腦要切換成日本語系，遇到基本鍵入的輸入法一直無法下載的問題，點擊了下載基本鍵入進度只會卡住然後就下載失敗，而且右下角會一直彈出訊息視窗顯示失敗一直循環

後來才發現是因爲開啟了從WSUS上攝取資源所導致的，只要暫時先把這個功能Disable後再點擊下載即成功，這個問題之前遇過不少次，每次都忘了做個紀錄XD，遇到時又要花時間再回想解決一次，這邊紀錄一下也分享給遇到相同問題的人


## Disable Use WSUS
```
@Echo Off
Net Stop WuAuServ
Reg Add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v "UseWUServer" /t "REG_DWORD" /d "0x00000000" /f
Del %SystemRoot%\SoftwareDistribution\*.* /S /Q
Net Start WuAuServ
```


## Enable Use WSUS
```
@Echo Off
Net Stop WuAuServ
Reg Add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v "UseWUServer" /t "REG_DWORD" /d "0x00000001" /f
Del %SystemRoot%\SoftwareDistribution\*.* /S /Q
Net Start WuAuServ
```
