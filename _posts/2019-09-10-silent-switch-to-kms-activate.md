---
layout: single
title: "靜默模式切換成KMS啟動Windows"
date: 2019-09-10
modified:
description:
categories:
    - "Tech Article"
tags:
    - Windows
    - KMS
header:
  image: https://i.imgur.com/RbYcXEP.png
---

現在的Windows，生命週期大大的縮短了，像是現在的Windows 10 1809，家用版、專業版、工作站專業版在2020年5月12日就終止服務了，但在企業版及教育版則能到2021年5月11日，所以最近就開始進行從專業版改為企業版的作業

一般來說KMS分為兩種方式在運作，一種會在SRV record中建立_vlmcs及使用Slmgr指定KMS Server的方式，今天的例子是以有建立SRV record的方式，只要客戶端在內部網路就能自動跟KMS Server報到

```vb
Slmgr /upk
Slmgr /ipk NPPR9-FWDCX-D2C8J-H872K-2YT43
Slmgr /ato
```
一般大家都是這種做法，但這種方式在大量部署時會有彈出視窗的問題，對於用戶來說會有莫名其妙的感覺，到時電話就準備接不完，所以我們可以使用vbs + bat的方式來做，當然要完全用vbs來做也是沒問題的，以下例子為使用vbs + bat


```vb
On Error Resume Next
key = CreateObject("WScript.Shell").RegRead("HKEY_USERS\s-1-5-19\")
If err.number <> 0 Then
WScript.Quit()
End If

Set objFSO = CreateObject("Scripting.FileSystemObject")
Set WshShell = CreateObject("WScript.Shell" )

If (objFSO.FileExists("\\172.17.100.116\User\KMS\KMS.bat")) Then
objFSO.CopyFile "\\172.17.100.116\User\KMS\KMS.bat", "C:\KMS.bat"
WshShell.Run chr(34) & "C:\KMS.bat" & Chr(34), 0 
Set WshShell = Nothing 
Else
WScript.Quit()
End If
```
首先確認權限是否具有Administrator的權限，如果沒有就退出程式，有的話複製KMS.bat到C:\底下並使用隱藏的方式執行


```vb
@Echo Off

:: Check System Version
:: https://docs.microsoft.com/en-us/windows-server/get-started/kmsclientkeys
For /f "tokens=4-5 delims=. " %%i in ('ver') Do Set Version=%%i.%%j
:: Windows 10
If "%Version%" == "10.0" Set KMS_Key=NPPR9-FWDCX-D2C8J-H872K-2YT43
:: Windows 8.1
If "%Version%" == "6.3"  Set KMS_Key=MHF9N-XY6XB-WVXMC-BTDCT-MKKG7
:: Windows 8
If "%Version%" == "6.2"  Set KMS_Key=32JNW-9KQ84-P47T8-D8GGY-CWCK7
:: Windows 7
If "%Version%" == "6.1"  Set KMS_Key=33PXH-7Y6KF-2VJC9-XBBR8-HVTHH

:: Check support status
If "%KMS_Key%" == "" (
    Goto :DelSelf
)

:: Uninstall Product key
cscript %SystemRoot%\System32\slmgr.vbs /upk > nul

:: Replace to KMS Key
cscript %SystemRoot%\System32\slmgr.vbs /ipk "%KMS_Key%" >nul

:: Activate Windows
cscript %SystemRoot%\System32\slmgr.vbs /ato >nul

:DelSelf
cls
Del /f /s /q %0
Exit
```
執行後會先進行確認Windows的版本是否支援，若是有支援的話就定義變數KMS_Key並賦值Public Key，接著如果判斷KMS_Key為空的話的話視為不支援即跳出程式並自我刪除

接下來就是移除目前的產品金鑰，替換成Public Key並與KMS Server報到啟用，這樣就完成KMS的啟動了，這個部署可以用GPO來做，或是用一些ITAM的工具來做也可以

另外比較需要注意的地方是，Windows版本的切換**只能在Windows 10 1703以上**，在這之前每個版本的系統核心是不一樣的，所以是無法換序號就切換版本的

參考資料：

[KMS Activation - Configuring DNS](https://docs.microsoft.com/en-us/previous-versions/tn-archive/ff793405(v=technet.10)?redirectedfrom=MSDN)

[Windows lifecycle fact sheet](https://support.microsoft.com/en-us/help/13853/windows-lifecycle-fact-sheet)

[KMS client setup keys](https://docs.microsoft.com/en-us/windows-server/get-started/kmsclientkeys)
