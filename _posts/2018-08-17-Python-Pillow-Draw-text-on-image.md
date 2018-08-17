---
title:  "Python 使用 Pillow (PIL) 對影像檔加入文字"
search: true
comments: true
categories:
  - Programming
tags:
  - Python
---

最近因應公司政策的關係，需要對Guest Wi-Fi做一些控管，所以當訪客需要Wi-Fi時會向櫃檯索取臨時性的連線資訊，當時我提案了可以將連線資訊印在標籤貼紙上，一式兩份，一份可以貼在訪客登記簿的備註欄，另一份則交給訪客使用，一來可以避免櫃檯登記錯誤；二來訪客使用也方便，甚至能暫時貼在筆電上頭，方便隨時查看輸入驗證。


提案過後當然就是後續的實作，因爲公司的AP設備並沒有相關的列印解決方案，所以我們一次可能生成二十組的臨時性連線資訊，接著匯出資訊成.txt檔案格式接著讓Python讀入檔案後解析連線資訊再產生標籤圖片做列印，即可達成提案需求

這個方式就必須依靠[Pillow (PIL)][Pillow (PIL)]來對影像做處理，影像處理是需要非常複雜的計算，但只要 Import PIL一切都簡單多了！

## Python: Install Pillow (PIL)
```console
pip install pillow
```

我們想對影像檔加入文字的話，只會使用到三個Module 分別是 [Image][Image], [ImageDraw][ImageDraw],  [ImageFont][ImageFont] 更詳細的用法，可以點擊連結查看，接著直接看一下範例如何實作。

```python
#Import Package
from PIL import Image, ImageDraw, ImageFont

#Input source image
Srcimg  = Image.open("src_img.png")

#Initialization & Use Srcimg as background
Drawimg = ImageDraw.Draw(Srcimg)

#Initialization & Use Truetype Font
Myfont  = ImageFont.truetype('myfont.ttf', size=30)

#Draw config
(x, y) = (10, 10) # Text position
color  = 'rgb(0, 0, 0)' # Text color
Mytext = 'Hello World!'

#Draw text on image
Drawimg.text((x, y), Mytext, fill=color, font=Myfont)

#Save image
Srcimg.save('HelloWorld.png')
```

## 運行結果圖
![None]({{ "https://i.imgur.com/fTpDRdK.png" | absolute_url }})

我在每行程式碼都有加上註解，然後要將.py、src_img.png、myfont.ttf放在同個目錄，.png(底圖)跟.ttf(字體)就是依個人所需可替換更改，就簡單的幾行就能達到需求，如果再加上迴圈就可以做到大量加入文字浮水印，非常的方便！

下次再來分享一下我是如何用Python + Pillow來實作 Wi-Fi 連線資訊標籤的印製～

[Pillow (PIL)]: https://pillow.readthedocs.io/
[Image]: https://pillow.readthedocs.io/en/5.2.x/reference/Image.html
[ImageDraw]: https://pillow.readthedocs.io/en/5.2.x/reference/ImageDraw.html
[ImageFont]: https://pillow.readthedocs.io/en/5.2.x/reference/ImageFont.html
