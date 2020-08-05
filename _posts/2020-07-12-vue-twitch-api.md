---
layout: single
title: "使用 Vue.js 串接 Twitch API 顯示熱門遊戲及直撥頻道"
date: 2020-07-12
modified:
description: "使用 Vue.js 串接 Twitch API 顯示熱門遊戲及直撥頻道"
categories:
  - "Tech Article"
  - "Front-End"
tags:
  - Vue
header:
  image: /assets/images/2020/07/12/2020-07-12-001.png
---

為甚麼突然想要練習串接 [Twitch API](https://dev.twitch.tv/docs/api) 呢？因爲突然想到之前閱讀過 [Huli](https://blog.huli.tw/) 提供免費前端開發教學實驗的[文章](https://blog.huli.tw/2017/06/03/frontend-tutorial-experiment/)裡面有提到串接 Twitch 的 API，是說當時一直想要報名，但是那時候公司的專案進行的如火如荼，實在沒有多餘的心力來學習，就遺憾的沒有報名了（飲恨

所以就因爲突然想到，就來練習看看怎麼接 Twitch 的 API，並展示出熱門的遊戲及熱門直撥頻道

<!-- Table of Contents -->

{% include toc icon="heart" title="使用 Vue.js 串接 Twitch API 顯示熱門遊戲及直撥頻道" %}

## 1. OAuth Token is Missing

當開心的開通了 Twitch Developer 拿到 token 後打開了 Postma 發出請求卻發現噴回了這個錯誤 "OAuth Token is Missing"，做了資料後發現原來是 Twitch 從 [2020/04/30](https://discuss.dev.twitch.tv/t/requiring-oauth-for-helix-twitch-api-endpoints/23916) 起，需要多一個 OAuth Token，所以這邊分享一下如何取得 OAuth Token

首先可以先編輯一下以下的參數，完成後直接用 Postman 發送請求出去，就能拿回一組 OAuth Token

```
POST https://id.twitch.tv/oauth2/token
    ?client_id=<your client ID>
    &client_secret=<your client secret>
    &grant_type=client_credentials
    &scope=<space-separated list of scopes>

Sample request:
https://id.twitch.tv/oauth2/token?client_id=uo6dggojyb8d6soh92zknwmi5ej1q2&client_secret=nyo51xcdrerl8z9m56w9w6wg&grant_type=client_credentials

{
    "access_token": "26pqfsvtrq7wylkvfsbgr9rc8e1860",
    "expires_in": 5656644,
    "token_type": "bearer"
}
```

上面的 access_token 只是範例，不是真的唷，這邊值得一提的是，OAuth Token 是有期限的，預設是 60 天，所以快過期之前可以再請求一次就可以續訂，或是等到過期之後直接再製作一個新 Token

## 2. Twitch API 使用方法

拿到 Oauth Token 後就可以測試看看能不能正常拿到資料，所以在發送請求要在 headers 中帶上 Client-ID 及 Authorization，範例如下

```
Client-ID : "Client-ID"
Authorization : "token_type" "access_token"

Client-ID : "123456"
Authorization : bearer 26pqfsvtrq7wylkvfsbgr9rc8e1860
```

Client-ID 就填上自己在 Twitch Developer 中的 ID，Authorization 則填上當請求 OAuth Token 回來的 token type 及 access_token 即可

## 3. Demo & Source Code

[Demo](https://vue-twitch-api.netlify.app/)

[Source Code](https://github.com/llovvoll/vue-twitch-api)

## 4. 開發過程

基本上這個練習是很簡單的作品，但是有幾個地方可以筆記一下

### 4-1. Vue Environment variable

當開發這種需要 Token 的應用程式，當然不可能把 Token 直接公開到程式碼之中，所以需要環境變數，在 Vue 使用環境變數時只要新增 ".env" 檔案即可，但建議這個檔案只要先填上變數名稱就可以了，因爲這個 ".env" 預設在 Vue CLI 中的 gitignore 是會被收錄的，所以我們可以在建立一個 ".env.local" 來儲存開發時需要使用的 Token ，當使用 "npm run serve" 的時候，會被自動載入且不會被 git 收錄進去，以下附上 Vue 的官方說明

```
.env                # loaded in all cases
.env.local          # loaded in all cases, ignored by git
.env.[mode]         # only loaded in specified mode
.env.[mode].local   # only loaded in specified mode, ignored by git
```

如果要使用環境變數的話，變數名稱必須是 "VUE_APP\_" 開頭，否則是無法使用的，範例如下

```
VUE_APP_TITLE=My App
```

讀取的方法 \"process.env.VUE_APP\_變數名稱"

```
console.log(process.env.VUE_APP_TITLE) // My App
```

Document : [Modes and Environment Variables](https://cli.vuejs.org/guide/mode-and-env.html#modes)

## 5. 總結

以上是這次練習串接 Twitch API 的小小心得。
