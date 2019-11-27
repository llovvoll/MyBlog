---
layout: single
title: "【Day 1】 甚麼是Microsoft Deployment Toolkit (MDT)"
date: 2019-11-21
modified:
description: "What is Microsoft Deployment Toolkit"
categories:
    - "Tech Article"
tags:
    - Windows
    - MDT
header:
  image: /assets/images/2019/11/21/2019-11-21-000.jpg
---

原本想以這個主題報名第11屆的IT鐵人賽，但想起來時已經截止了Orz....，但是沒關係，我決定在自己的部落格上自我挑戰，在哪裡挑戰都不是問題，主要是挑戰自己的意志力嘛XD，這個也是我第一次參加30天鐵人賽的活動，希望可以順利達成！

首先來談談為甚麽我會想介紹MDT，MDT是個很方便的系統部署工具，它可以幫助IT節省許多時間及增加效率，你可以透過一些腳本去達到自動化部署，但是要達到自動化必須針對所需的環境寫一些腳本(Batch、PoweShell)，所以這也是許多IT認為使用MDT很麻煩的地方，而且MDT時常會有些很特殊的狀況必須是嘗試排解，當初我也花了不少時間在Debug，而且不知道是不是使用MDT太過麻煩，其實網路上的文獻並不多，所以只能靠自己花時間來摸索排除問題，這也是為甚麼我想介紹這個主題的原因了

現在企業中許多IT在部署系統，大多還是依靠Ghost或Acronis這類的工具去包成一個映像檔，每次要安裝或重灌時就一台一台去還原，這個方式的確快速省時，但電腦型號一多時，維護時Loading就出現了，可能因爲系統要更新、應用程式要更新、系統設置要更新，這時就要重新再打包一次映像檔，而且又有可能手誤或是不同人的關係導致每次的映像檔不一致，而且其實這個還有一個最糟糕的地方，每台的系統識別碼(SID)會是重復的，這對於企業環境中其實是不好的，它可能會影響到一些應用程式的運作及WSUS的一些功能運作，雖然在Acronis有清洗SID的功能，但在Windows10上其實是存在一些問題的，建議還是別使用這類工具去清洗SID，所以微軟的建議必須先做Sysprep的一般化處理，讓解封裝時產生新的SID確保之後不會發生一些奇奇怪怪的狀況

以上諸如此類的問題我都曾走過，所以後來我選擇花了一段時間來導入MDT，例如現在我要開始部署Windows 10 1909，以往我要開始針對幾種不同的型號重新打包一份新的映像檔，少說花上半天的時間，現在我只要針對我的MDT Task Sequences點擊個幾下，30秒不到的時間，我已經可以開始部署最新版本的系統到電腦上頭，這麼方便的工具還能不用嗎？

# Ｗhat is Microsoft Deployment Toolkit (MDT)
> Microsoft Deployment Toolkit (MDT) 自 2003 年起已經存在，它最初是以 Business Desktop Deployment (BDD) 1.0 形式引進的。 此工具組是以同時兼具功能性與常用性的形式逐步開發，而現今已被視為是 Windows 作業系統與企業應用程式部署的基礎。

簡單來說，MDT最主要功能就是系統部署，你可以做到零接觸安裝(ZTI)、精簡接觸安裝(LTI)、互動式安裝(UDI)，你可以自定義部署的工作任務(Task Sequences)，選擇要安裝哪個系統、驅動程式、系統更新、應用程式，甚至是管理員密碼或是產品金鑰，經由MDT Server，當你使用Windows PE Boot進到MDT時便會通過網路與MDT Server取得工作任務，並依所定義任務流程進行部署

MDT是個很深奧的工具，你可以簡單的運用，也可以做到很深層的運用，就看何去使用，下一篇我們就開始安裝MDT，開始動手玩玩吧～

![]({{ site.url }}/assets/images/2019/11/21/2019-11-21-001.png)

## References
[Microsoft Deployment Toolkit - Wikipedia](https://en.wikipedia.org/wiki/Microsoft_Deployment_Toolkit)

[Key features in MDT (Windows 10) - Microsoft Docs](https://docs.microsoft.com/en-us/windows/deployment/deploy-windows-mdt/key-features-in-mdt)