---
title:  "Dell iDRAC9 設定 SMTP Service & Email Alert 功能"
search: true
comments: true
header:
  image: https://i.imgur.com/PVvQtPV.png
categories:
  - Teaching
tags:
  - iDRAC
---

Dell Remote Access Controller或DRAC是一個基於頻帶外（out-of-band management）對伺服器進行管理的平台，由Dell公司所開發。該平台可以是外插卡或者是以晶片的形式出現，以晶片的形式則稱之為iDRAC。

DRAC曾是Adrian White的發明專利，其提供基於瀏覽器（browser-based）或是指令列（command-line）的兩種介面選項，可以兩者擇一或是兩者兼具，對於伺服器的硬體進行管理與監控。
[<資料來源：https://zh.wikipedia.org/wiki/Dell_DRAC>][wiki_Dell_DRAC]


有接觸過iDRAC的朋友們一定對它愛不釋手，建議如果你的Dell Server支援這項功能一定啟用它，這對後續的維護有非常大的效益！
今天來介紹一下如何在iDRAC9中設定SMTP功能及開啟Email Alert功能，當Server故障時才能即時發出Alert給相關owner，以免服務中斷過久沒人處理...到時候就有人準備被電到飛起來了XD


## 設定 iDRAC DNS Name

登入後先點選 iDRAC Settings > Common Settings ，接著輸入一些基本資料，DNS iDRAC Name 這邊輸入Server Name或是你想要顯示的名稱，Static DNS Domain Name的話就是你的domain name，當我如圖中的資訊的話，那我Email address 就會是"ABCSRV@ovvo.cc"

![None]({{ "https://i.imgur.com/HF6arc8.png" | absolute_url }})


## 開啟 Alert & Email Alert

接著跳到 Configuration > System Settings， 先將 Alert Configuration 選項 Enabled 啟用，接著展開 Alerts and Remote System Log Configuration， 將需要的發出警報訊息類型做設定

![None]({{ "https://i.imgur.com/qHkLKrx.png" | absolute_url }})


## 設定 SMTP Service

SMTP (Email) Configuration 這邊的欄位就填入需要收到Email Alert的owner並記得將 State 打勾啟用，SMTP (Email) Server Settings 則是將 自己的 SMTP Server IP相關資訊帶入接著Apply儲存，接著試試按Send看有沒有收到Test Email即可！


![None]({{ "https://i.imgur.com/oZNqFn5.png" | absolute_url }})






[wiki_Dell_DRAC]: https://zh.wikipedia.org/wiki/Dell_DRAC
