---
layout: single
title: "Vue.js 新手上路之初體驗筆記"
date: 2019-12-19
modified:
description: "Vue.js 新手上路之初體驗筆記"
categories:
    - "Tech Article"
    - "Front-End"
tags:
    - Vue
header:
  image: /assets/images/2019/12/19/2019-12-19-000.png
---

人在江湖飄 哪能不挨刀，所欠缺的技術債總有一天該還的，所以就趕緊的來學學這好用的前端框架Vue.js吧，這篇不是篇稱職的教學文章，只是紀錄一下筆記，好讓自己內化一下記憶，如果有錯誤的地方還請多多包涵

<!-- Table of Contents -->
{% include toc icon="heart" title="Vue.js 新手上路之初體驗筆記" %}


[Vue.js - Development Version](https://vuejs.org/js/vue.js)

[Vue.js - Production Version](https://vuejs.org/js/vue.min.js)

首先要使用Vue.js的話，可以直接在html中引入CDN的vue.js或是直接下載官方網站的.js檔案，開發或學習時建議使用開發版本，開發版本有完整的警告和測試模式，當要正式部署時可以更換為生產版本，檔案已經壓縮過而且去除了警告的部分，這樣比較節省流量

<!-- {% raw %} -->
```html
<!-- Vue.js - Development Version from CDN -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
<!-- {% endraw %}) -->

引入Vue.js後就可以開始使用，首先可以先建立一個app的Vue函數來使用並將data中的msg渲染到網頁上
<!-- {% raw %} -->
```html
<div id="app>
    {{ msg }}
</div>

    var app = new Vue ({
        el: "#app",
        data: {
            msg: "Hello World!"
        }
    })
```
<!-- {% endraw %}) -->

這邊的el是element的縮寫，這是元素選擇器，用法跟jQuery一樣，要選擇ID使用#開頭，要選擇class的話則使用.開頭，Data的是Vue.js進行儲存元件狀態或是資料，裡面可以object或是function，這邊要注意的是**任何命名不可以使用$或_所開頭的名稱，這是Vue.js所保留的，當Vue遇到$或_是不會進行處理的**



## v-model (資料雙向綁定)
v-model主要用在表單元件或是自訂元件，常用在input、select、textarea上，範例如下
<!-- {% raw %} -->
```html
<div id="app">
    <input type="text v-model="msg">
    Output: {{ msg }}
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: "Hello World!"
    }
})
```
<!-- {% endraw %}) -->
當進入網頁時input這時的值會是Hello World!，Output亦同，當input文字異動時，msg也會即時同步更新

### v-model.number
v-model預設型別為string，使用v-mode.number時可將數字強制轉換為number型別
<!-- {% raw %} -->
```html
<div id="app">
    <input type="text" v-model.number="msg">
    Output: {{ typeof msg }}
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: "Hello World!"
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/guide/forms.html#number](https://vuejs.org/v2/guide/forms.html#number)

### v-model.lazy
v-model預設為監聽input事件，使用.lazy方法時可以改為監聽change事件，所以當在input輸入時並不會更新資料，但當滑鼠點擊其它元素離開了input或Enter，這時資料就會進行更新
<!-- {% raw %} -->
```html
<div id="app">
    <input type="text" v-model.lazy="msg">
    Output: {{ msg }}
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: "Hello World!"
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/guide/forms.html#lazy](https://vuejs.org/v2/guide/forms.html#lazy)

### v-model.trim
v-model.trim可以去除開頭及結尾的空白字元
<!-- {% raw %} -->
```html
<div id="app">
    <input type="text" v-model.trim="msg">
    Output: {{ msg }}
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: "Hello World!"
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/guide/forms.html#trim](https://vuejs.org/v2/guide/forms.html#trim)

## v-on:[event] (事件監聽器)
v-on是個事件監聽器，常用於監聽click事件，v-on也有許多的方法可以使用，這邊會介紹三個較常用的方法，其它可以參考底下的官方文件，v-on基本範例如下
<!-- {% raw %} -->
```html
<div id="app">
    <button v-on:click="sayHi"></button>
    <!-- 可以將v:on縮寫成@使用 -->
    <button @click="sayHi"></button>
    {{ msg }}
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: ""
    },
    methods: {
        sayHi: function() {
            this.msg = "Hi"
        }
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/api/#v-on](https://vuejs.org/v2/api/#v-on)

### v-on:[event].stop
接著介紹一下較常用的v:on方法，stop可以阻止事件冒泡，範例如下，如果不在button加上stop，上層的div也會一同觸發
<!-- {% raw %} -->
```html
<div id="app" v-on:click="sayHi('From div')">
    <button v-on:click.stop="sayHi('From button')"></button>
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: ""
    },
    methods: {
        sayHi: function(text) {
            this.msg = text
        }
    }
})
```
<!-- {% endraw %} -->

### v-on:[event].prevent
prevent可以防止執行預設的行為，用法可以帶參數或不帶參數僅停用預設的事件，範例如下
<!-- {% raw %} -->
```html
<div id="app">
    <a href="https://vuejs.org/v2/api/#v-on">點我會跳轉</a>
    <a href="https://vuejs.org/v2/api/#v-on" v-on:click.prevent="sayHi">點我不會跳轉</a>
    <a href="https://vuejs.org/v2/api/#v-on" v-on:click.prevent>點我不會跳轉</a>
    {{ msg }}
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: ""
    },
    methods: {
        sayHi: function() {
            this.msg = "偵測到click.prevent事件，停用預設的事件"
        }
    }
})
```
<!-- {% endraw %} -->

### v-on:[event].capture
capture是事件捕獲，與事件冒泡的順序相反，事件捕獲是由外到內，事件冒泡則是由內到外

事件捕獲： outer -> middle -> inner

事件冒泡： inner -> middle -> outer
<!-- {% raw %} -->
```html
<div id="app">
    <div @click.capture="outer">
        <div @click.capture="middle">
            <button @click.capture="inner">click me</button>
        </div>
    </div>
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: ""
    },
    methods: {
        outer: function() {
            alert("outer")
        },
        middle: function() {
            alert("middle")
        },
        inner: function() {
            alert("inner")
        }
    }
})
```
<!-- {% endraw %} -->

## v-bind:[attribute] (屬性綁定)
如果綁定HTML的屬性(Attribute)，可以使用v-bina:[attribute]，有一點要注意的地方是，如果HTML及v-bind的屬性同時存在的話，v-bind是優先的，原先的HTML屬性會失效，接著來看個簡單的範例吧
<!-- {% raw %} -->
```html
<div id="app">
    <a href="https://vuejs.org/v2/api/#v-on" v-bind:href=url>點我會跳轉</a>
    <!-- v-bind可以使用:當作縮寫 -->
    <a href="https://vuejs.org/v2/api/#v-on" :href=url>點我會跳轉</a>
    </div>

var app = new Vue ({
    el: "#app",
    data: {
        url: "https://google.com"
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/api/#v-bind](https://vuejs.org/v2/api/#v-bind)

## v-if、v-else if、v-else
Vue.js可以使用條件式來控制渲染元素，其用法跟JavaScript一樣，簡單的範例如下
<!-- {% raw %} -->
```html
<div id="app">
    <p v-if="money >= 60">你有{{ money }}塊, 吃麥當勞</p>
    <p v-else-if="money >= 30">你有{{ money }}塊, 吃炸雞排</p>
    <p v-else>你只有{{ money }}塊, 只能吃土</p>
</div>

var app = new Vue ({
    el: "#app",
    data: {
        money: Math.floor(Math.random()*100) + 1
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/api/#v-if](https://vuejs.org/v2/api/#v-if)

## v-for
v-for可以用來運行迴圈並渲染元素，首先是陣列(Array)的部分，基本上跟JavaScript一樣的操作方式
<!-- {% raw %} -->
```html
<div id="app">
    <div v-for="(item, index) in items">
        {{ item }}
        {{ item.name}}
        {{ item.age}}
    </div>
</div>

var app = new Vue ({
    el: "#app",
    data: {
        items:[
        { name: 'abc', age: 18 },
        { name: 'ppp', age: 20 },
        { name: 'ccc', age: 30 }
        ]
    }
})
```
<!-- {% endraw %} -->

物件(Object)的使用範例如下
<!-- {% raw %} -->
```html
<div id="app">
    <div v-for="(item, key, index) in videoList">
        {{ item }}
        {{ key }}
        {{ item.videoDate}}
    </div>
</div>

var app = new Vue ({
    el: "#app",
    data: {
        videoList: {
            'ABC': { videoDate: '2019' },
            'BBB': { videoDate: '2015' },
            'CCC': { videoDate: '2005' }
        }
    }
})
```
<!-- {% endraw %} -->
[https://cn.vuejs.org/v2/api/#v-for](https://cn.vuejs.org/v2/api/#v-for)

## v-html
**Danger Notice:** 在網站上動態渲染任意HTML是非常危險的，因為容易導致XSS攻擊。只在可信內容上使用v-html，永不在用戶提交的內容上。
{: .notice--danger}
<!-- {% raw %} -->
```html
<div id="app" v-html="myhtml">
</div>

var app = new Vue ({
    el: "#app",
    data: {
        myhtml: '<h1> Hello World! </h1>'
    }
})
```
<!-- {% endraw %} -->
## v-once
這個能讓元素只渲染一次，之後的所有更動都將不會進行渲染
<!-- {% raw %} -->
```html
<div id="app">
    <p v-once>{{ msg }}</p>
    <p>{{ msg }}</p>
    <button @click="sayHi">click me</button>
</div>

var app = new Vue ({
    el: "#app",
    data: {
        msg: 'First'
    },
    methods: {
        sayHi: function() {
            this.msg = 'SayHi'
        }
    }
})
```
<!-- {% endraw %} -->
[https://vuejs.org/v2/api/#v-once](https://vuejs.org/v2/api/#v-once)

## References
[API — Vue.js](https://vuejs.org/v2/api/#Directives)

[vue.js directives - Summer。桑莫。夏天](https://cythilya.github.io/tags/vue.js%20directives/)