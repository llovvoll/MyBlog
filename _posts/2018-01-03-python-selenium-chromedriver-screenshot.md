---
layout: single
title: "Python+Selenium配合ChromeDriver 自動化登入&截圖"
date: 2018-01-03
modified:
description:
categories:
    - "Tech Article"
tags:
    - Python
    - Selenium
header:
---

2018年的第一篇文章，祝各位新年快樂！
因爲公司的系統有些老舊，一些功能細節做的實在不夠自動化，會造成許多的Loading，所以選了Python來做一些自動化的作業
今天文章中的範例是將ITHome首頁截圖下來，並做簡易的自動登入再次截圖。


## Python: Install [Selenium][Selenium]
首先先安裝Selenium，下「pip install selenium」指令將其安裝起來，如果在macOS下指令報錯誤的話，可能是沒有安裝pip，可以使用「sudo easy_install pip」安裝後再用pip安裝selenium應該就沒問題了。

## Install [Chrome Driver][Chrome driver]
這邊我們用的Browsers driver 是使用Chrome，如果有特殊需求的話可以依上面的連結裝起來，macOS的話可以利用Homebrew把Chrome Driver裝起來，「brew install chromedriver」，裝好後就可以開始來寫程式了。


```python
from selenium import webdriver

Browser = webdriver.Chrome()
WebUrl  = ('https://www.ithome.com.tw/')
Browser.get(WebUrl)
Browser.save_screenshot('test.png')
Browser.quit()
```
Python的好用之處...就是在資源庫非常的多，只要導入Package，就可以輕易達成許多功能，像這個截圖就只花了七行，沒錯就是七行沒有看錯，就是如此輕易！

再來增加一點點複雜度，我們要做到自動登入ITHome後並截圖個人設定頁面，先看以下的範例程式碼
```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

Browser = webdriver.Chrome()
LoginUrl= ('https://member.ithome.com.tw/login')
UserName= ('#####')
UserPass= ('#####')

Browser.get(LoginUrl)
Browser.find_element_by_id('account').send_keys(UserName)
Browser.find_element_by_id('password').send_keys(UserPass)
Browser.find_element_by_id('password').send_keys(Keys.ENTER)
Browser.save_screenshot('test.png')
Browser.quit()
```

這邊我們多導入了一個Package，並利用Selenium的內建函數去搜尋元素ID為Account及password的輸入欄位，以下是擷取ITHome登入頁面的Html code
![None]({{ "https://i.imgur.com/aQakHug.png" | absolute_url }})

```html
<div class="form-group has-feedback has-feedback-left">
			    <label class="sr-only">帳號</label>
			    <input id="account" class="form-control" placeholder="帳號(非email)" name="account" type="text">
				<i class="fa fa-user form-control-feedback"></i>
			</div>
      <div class="form-group has-feedback has-feedback-left">
			    <label class="sr-only">密碼</label>
			    <input id="password" class="form-control" placeholder="密碼" name="password" type="password" value="">
				<i class="fa fa-lock form-control-feedback"></i>
			</div>
```
[Selenium Locating Elements Document][Selenium Locating Elements Document]

利用find_element_by_id查找出指定的元素，並用send_keys鍵入我們已建立的帳號密碼變數，最後並在password的欄位送出Enter鍵做為登入
登入完畢則利用save_screenshot將畫面存取為test.png，這邊則無指定路徑，所以會將其儲存在此Python程式的相同目錄下，最後完成工作後關閉Browser

![None]({{ "https://i.imgur.com/UU3sLn4.png" | absolute_url }})


以上是簡略的介紹，如果有不明白的地方可以留言一起討論交流

[selenium]: https://pypi.python.org/pypi/selenium
[Chrome driver]: http://chromedriver.chromium.org
[Selenium Locating Elements Document]: https://selenium-python.readthedocs.io/locating-elements.html#
