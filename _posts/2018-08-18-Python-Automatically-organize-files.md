---
title:  "利用 Python 自動整理及分類檔案"
search: true
comments: true
categories:
  - Programming
tags:
  - Python
---

昨天與朋友吃完飯，朋友提出了個需求，是需要利用關鍵字去做檔案的整理及分類，所以回家就幫他寫了一下，也當作一個練習，這邊也分享一下如何利用Python來做到檔案的自動分類。

為了達到這個需求，我們需要import幾個Package，分別是 [shutil][shutil]、[os][os]、[os.walk][os.walk]、[os.path.join][os.path.join]，一樣想要查看詳細使用方法可以點擊連結查看。

這幾個Package都是Python所內建的，所以不需要特別安裝，直接引用即可，我們就直接看一下範例如何實作

```python
import shutil
import os
from os import walk
from os.path import join

MyPath  = './' # 當下目錄
KeyWord = input = '請輸入檔案關鍵字:'

for root, dirs, files in walk(MyPath):
  for i in files:
    FullPath = join(root, i) # 獲取檔案完整路徑
    FileName = join(i) # 獲取檔案名稱

    if KeyWord in FullPath:
      if not os.path.exists(MyPath + '/' + KeyWord + '/' + FileName):
        if not os.path.exists(KeyWord):
          os.mkdir(KeyWord)
        shutil.move(FullPath, MyPath + '/' + KeyWord)
        print ('成功將', FileName, '移動至', KeyWord, '資料夾')
      else:
        print (FileName, '已存在，故不執行動作')
```

前六行基本上一看就知道甚麽意思，直接看一下迴圈的部分，先用迴圈遞歸搜尋檔案的完整路徑和名稱，並把完整路徑和檔案名稱寫入FullPath變數及FileName變數中

接著在第二層迴圈中加入條件判斷，當KeyWork有符合在FullPath的話就繼續執行，第二層判斷是判斷檔案是否存在，若不存在時就繼續下一步，反之存在的話就是顯示已存在的訊息，跳出這次的迴圈

再來若是判斷不存在的話就接著判斷是否有KeyWord名稱的資料夾，若沒有的話就創建一個KeyWord名稱的資料夾，接著就是使用shutil.move方法去搬移我們的檔案到資料夾中

## 運行結果圖
![None]({{ "https://i.imgur.com/9LdWkgK.png" | absolute_url }})

使用起來起來也是滿方便，關鍵字的輸入並沒有限制檔案名稱而已，想要輸入.png也是可以的，所以其實想要拿來自動分類圖片類型或是整理特定關鍵字的檔案都很方便，這種將程式融入於生活中其實挺不賴的！

另外雖然程式寫的滿糟糕的，但還是能達到我們的需求，如果有更好的方法歡迎交流指點 :)

[shutil]: https://docs.python.org/3/library/shutil.html
[os]: https://docs.python.org/3/library/os.html
[os.walk]: https://docs.python.org/3/library/os.html#os.walk
[os.path.join]: https://docs.python.org/3/library/os.path.html#os.path.join
