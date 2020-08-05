---
layout: single
title: "使用 Vue.js + Element UI 製作 Todo List 全紀錄"
date: 2020-07-10
modified:
description: "使用 Vue.js + Element UI 製作 Todo List 全紀錄"
categories:
  - "Tech Article"
  - "Front-End"
tags:
    - Element
    - Vue
---

這是第一個用 Vue 搭配 [Element UI](https://element.eleme.io/) 寫出 CRUD 功能的練習作品，這篇文章紀錄一下開發過程遇到的問題及解決方式，程式的邏輯都是自己設計出來....或許很多地方都不是正規做法，請多多包涵 Orz...

<!-- Table of Contents -->

{% include toc icon="heart" title="使用 Vue.js + Element UI 製作 Todo List 全紀錄" %}

## Demo & Sorece Code

[Demo](https://my-vue-to-do-list.netlify.app/)

[Source Code](https://github.com/llovvoll/vue-to-do-list)

## 1. 規劃 User Story（用戶故事）

建議在動手寫程式之前，先稍微規劃一下程式的規格，可以避免開發時毫無方向造成卡卡的狀況，所以首先我們可以先以使用者的立場去設計一個 User Story

- 身為使用者，我可以新增待辦事項
- 身為使用者，我可以編輯或刪除待辦事項
- 身為使用者，我可以將待辦事項設定成已完成
- 身為使用者，我可以將待辦事項設定為重要(星號標示)
- 身為使用者，我可以篩選顯示出已完成或未完成的待辦事項
- 身為使用者，我可以儲存所有資料，下次使用時不需要重新輸入

這樣子在開發時就不會失了方向，而且可以將大的項目拆分成小項目，這樣逐漸開發起來也比較不會備感壓力

## 2. Data structure（資料結構）

當設計完用戶故事，這時對於整個程式架構就會有大概的方向，我們就可以來設計程式所需使用到的資料結構，這樣也不會導致開發時隨便設定了許多變數塞到資料中，寫到後面可能自己也忘記是甚麼了

```javascript
// [ {uuid: _uuid(), title: "string", date: "string", comments: [ "String" ], isComplete: boolean, isImportant: boolean} ]
eventList = [{}];
```

## 3. Fake Data（假資料）

開發這種互動性較高的網頁，最好是先加入假資料，這樣對於後續的開發也比較方便

```javascript
    fakeData() {
      for (let i = 0; i < 10; i++) {
        this.eventList.push({
          uuid: this._uuid(),
          title: Math.floor(Math.random() * 1000),
          date: getDate(true),
          comments: ["Hello World", "Happy Vue.js"],
          isComplete: Boolean(Math.round(Math.random())),
          isImportant: Boolean(Math.round(Math.random()))
        });
      }
    }
```

## 4. 新增待辦事項功能

剛開始的寫法是將新增與編輯的 Function 拆分開來，但後來發現程式碼根本快一模一樣，所以這邊塞了一個 isEdit 變數，做為判斷前端來的行為是新增或編輯，在這邊我使用了變數 temporary 來儲存使用者新增待辦事項所輸入的資料並使用 [Array.push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 將資料儲存到 eventList 中

```javascript
    eventSave() {
      if (this.isEdit) {
        // Edit Mode
        let index = this.editTmp.target;
        this.eventList[index].title = this.editTmp.title;
        this.eventList[index].comments = this.editTmp.comments;
        this.editTmp = {};
        this.isEdit = false;
      } else {
        // Add Mode
        this.eventList.push({
          uuid: this._uuid(),
          title: this.temporary.title,
          date: getDate(true),
          comments: null,
          isComplete: false,
          isImportant: false
        });
        this.temporary = {};
      }
    },
```

前端進行儲存資料，呼叫 eventSave() 的使用方法，在 Click 事件上直接呼叫即可，這邊值得一提的是，當使用 \<el-input> 想監聽 keyup.enter 事件時必須使用 [@keyup.enter.native="eventSave"](https://github.com/ElemeFE/element/issues/2333) 而不是 @keyup.enter

```html
<el-input
  v-model="temporary.title"
  placeholder="Event Title"
  @keyup.enter.native="eventSave"
></el-input>

<el-button type="primary" size="mini" round @click="eventSave">Save</el-button>
```

## 5. 刪除待辦事項功能

這邊是很簡單的讀取從前端傳回來的指定陣列長度並使用 [Array.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 達到刪除的功能，這邊要注意不要拼錯成 [Array.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) 當時沒注意到，刪了老半天沒反應，結果是拼錯字 Orz..

```javascript
    eventDel(index) {
      this.eventList.splice(index, 1);
    }
```

前端使用 [v-for](https://vuejs.org/v2/guide/list.html) 來渲染出待辦事項 eventList 並將 index 綁定每個刪除按鈕 click 事件上

```html
<div class="eventList">
  <div class="eventItem" v-for="(item, index) in eventList" :key="item.uuid">
    <el-button
      type="danger"
      size="mini"
      icon="el-icon-delete"
      @click="eventDel(index)"
    ></el-button>
  </div>
</div>
```

## 6. 編輯待辦事項功能

這邊要把 eventSave() 放在一起來看會比較清晰一點，原本是把新增與編輯分開來寫，但是可以發現程式碼其實很相像，所以就一起寫進了 eventSave() 中

首先當使用者點擊了編輯按鈕時會呼叫 eventEdit(index) 並傳入當前編輯項目的陣列 index，此時 isEdit 變數改變成 true，將 Dialog 編輯視窗呼叫出來，並將 editTmp 使用 [Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 拷貝一份 eventList[index] 到 editTmp 中，這邊拷貝的用意是準備在編輯頁面中渲染出需編輯項目的資料，並將傳入的 index 賦值到 editTmp.target 中

當使用者按下儲存按鈕會呼叫 eventSave() 儲存編輯資料時會利用 editTmp.target 的 index 賦值到真正的 eventList[index] 中，當儲存完資料時將 editTmp 清空並把 isEdit 改變成 false 關閉 Dialog 編輯視窗

```javascript
eventSave() {
      if (this.isEdit) {
        // Edit Mode
        let index = this.editTmp.target;
        this.eventList[index].title = this.editTmp.title;
        this.eventList[index].comments = this.editTmp.comments;
        this.editTmp = {};
        this.isEdit = false;
      } else {
        // Add Mode
        this.eventList.push({
          uuid: this._uuid(),
          title: this.temporary.title,
          date: getDate(true),
          comments: null,
          isComplete: false,
          isImportant: false
        });
        this.temporary = {};
      }
    },
    eventEdit(index) {
      this.isEdit = true;
      this.editTmp = Object.assign({}, this.eventList[index]); // 利用Object.assign將要修改的的物件複製一份出來，若使用賦值方式就變成同步即時修改了
      this.editTmp.target = index; // 加入欲修改目標的index，好讓eventSave()調用
    }
```

前端點擊編輯按鈕並呼叫 eventEdit(index)，將 isEdit 變數改變成 true 呼叫出 Dialog 編輯視窗，在編輯視窗中抓取 editTmp 的資料渲染到畫面中

```html
<el-button
  type="primary"
  size="mini"
  icon="el-icon-edit"
  @click="eventEdit(index)"
></el-button>

<template>
  <el-dialog title="Edit" center :visible.sync="isEdit" :width="dialogWidth">
    <el-form :model="editTmp">
      <el-form-item label="Title" :label-width="formLabelWidth">
        <el-input v-model="editTmp.title"></el-input>
      </el-form-item>
      <el-form-item label="Comments" :label-width="formLabelWidth">
        <el-input
          v-model="editTmp.inputComment"
          placeholder="Add Comment..."
          v-if="!editTmp.comments"
          @keyup.enter.native="editTmp.comments = [editTmp.inputComment]"
        ></el-input>
        <el-input
          v-model="editTmp.inputComment"
          placeholder="Add Comment..."
          v-if="editTmp.comments"
          @keyup.enter.native="editTmp.comments.push(editTmp.inputComment)"
        ></el-input>
      </el-form-item>
      <div
        class="dialog-comment"
        v-for="(item, index) in editTmp.comments"
        :key="index"
      >
        <label class="el-icon-s-comment">{{ item }}</label>
        <el-tooltip
          effect="dark"
          content="此更動為即時刪除，請確認！"
          placement="right"
        >
          <el-button
            type="danger"
            size="mini"
            icon="el-icon-delete"
            @click="editTmp.comments.splice(index, 1)"
          ></el-button>
        </el-tooltip>
      </div>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="isEdit = false">Cancel</el-button>
      <el-button type="primary" @click="eventSave">Save</el-button>
    </div>
  </el-dialog>
</template>
```

## 7. 待辦事項 完成/ 未完成/ 重要(星號) 狀態變更功能

這個功能相對單純簡單，只是利用了 [v-model](https://vuejs.org/v2/guide/forms.html) 雙向綁定配合 [v-for](https://vuejs.org/v2/guide/list.html) 就可以達成功能

```html
<div class="eventItem" v-for="(item, index) in eventList" :key="item.uuid">
  <div class="eventItem-title">
    <!-- 完成/未完成 -->
    <el-checkbox v-model="item.isComplete"></el-checkbox>

    <!-- 重要(星號) -->
    <el-button
      type="primary"
      size="mini"
      icon="el-icon-star-off"
      v-if="!item.isImportant"
      @click="item.isImportant = true"
    ></el-button>
    <el-button
      type="primary"
      size="mini"
      icon="el-icon-star-on"
      v-if="item.isImportant"
      @click="item.isImportant = false"
    ></el-button>
  </div>
</div>
```

## 8. 篩選待辦事項功能

這個功能相對複雜了一點 (因爲自己太菜了...)，剛開始寫這個功能是是使用類似編輯待辦事項時的寫法，先將 eventList 暫存出來，雖然可以達成功能，但是會延伸出很多問題，例如當你篩選顯示已完成的項目，雖然渲染成功，但是會無法儲存編輯與刪除，為甚麼？因爲數據源不一樣，顯示的只是被暫存出來的，當時解決了 A 問題又延伸了 B...C 問題，所以我覺得寫法有問題，後來想到直接從前端渲染來解決這個問題，既可以正常顯示又能正常編輯刪除

因爲前端畫面我拆分為兩個組件，分別為 Header 及 Main，篩選按鈕是在 Header 的部分，所以這牽涉到跨組件狀態的問題，所以這邊有使用到一點點 [vuex](https://vuex.vuejs.org/) 來處理狀態問題

首先先在 store/index.js 中建立篩選模式的變數，並在 mutations 中加入 setViewMode() Function，用來變更 viewMode 變數，這邊要特別注意的是在 vuex state 的狀態唯一變更的方式是使用 [store.commit](https://vuex.vuejs.org/guide/mutations.html) 來提交變更，實際用法可以點擊連結看一下官方文件

```javascript
// @/store/index.js
export default new Vuex.Store({
  state: {
    viewMode: "ALL", // ALL, PRO, COM
  },
  mutations: {
    setViewMode(state, mode) {
      state.viewMode = mode;
    },
  }
```

在前端 Header 頁面中加入按鈕讓使用者點擊篩選模式，並呼叫 setViewMode() 來變更狀態

```html
<template>
  <div class="header">
    <el-row type="flex" justify="center">
      <el-col :lg="24">
        <el-menu
          :default-active="activeIndex"
          class="el-menu-custom"
          mode="horizontal"
          background-color="#4A90E2"
          active-text-color="#FFFFFF"
          text-color="#00408B "
        >
          <el-menu-item index="1" @click="setViewMode('ALL')"
            >My Tasks</el-menu-item
          >
          <el-menu-item index="2" @click="setViewMode('PRO')"
            >In Progress</el-menu-item
          >
          <el-menu-item index="3" @click="setViewMode('COM')"
            >Completed</el-menu-item
          >
        </el-menu>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: "Header",
    data() {
      return {
        activeIndex: "1",
      };
    },
    methods: {
      setViewMode(mode) {
        this.$store.commit("setViewMode", mode);
      },
    },
  };
</script>
```

當篩選模式選擇完之後，我們回來看一下主畫面，這邊就相對單純了，我們直接在前端使用 v-if 來判斷篩選模式並選擇性渲染就能完成篩選的功能

```html
<div v-if="this.$store.state.viewMode == 'ALL'">....</div>
<div v-if="this.$store.state.viewMode == 'PRO'">
  <div class="eventList" v-if="!item.isComplete">
    . . .
  </div>
</div>
<div v-if="this.$store.state.viewMode == 'COM'">
  <div class="eventList" v-if="item.isComplete">
    . . .
  </div>
</div>
```

這段程式碼比較又臭又長...有興趣的話可以[點我](https://github.com/llovvoll/vue-to-do-list/commit/ae0d69e59e437ee09f28f97440a6a025f96025d9)看一下完整版，這段我覺得可以再優化改寫，但這是我目前想到的最好方法且邏輯比較清晰簡單，重點是避免功能壞掉的做法 XD

## 9. 儲存待辦事項功能

因爲沒介接到任何資料庫，所以這邊我使用 [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) 來進行實現功能

### 9-1. 儲存資料

因爲 localStorage 只能使用純文字格式來儲存資料，所以需要將 eventList 使用 [JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉為 JSON 格式來儲存

這邊值得一提的是，因爲我們的 eventList 變數是陣列其中又包含著物件，若是直接用 [watch](https://vuejs.org/v2/guide/computed.html) 來追蹤 eventList 時會無法追蹤到底層的變動，例如使用者新增了新的 comment 或是將待辦項目狀態改為已完成，這時因爲追蹤不到，導致資料沒有儲存

所以這邊使用了 [computed](https://vuejs.org/v2/guide/computed.html) 計算屬性，在其中我們使用了 [Array.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 將 eventList 中的指定元素組成新陣列並 return，[computed](https://vuejs.org/v2/guide/computed.html) 特性是當資料有任何變動時都會進行計算，所以我們利用這個特性及搭配 watch 來追蹤 eventList 是否有任何變動，若有就即時儲存到 localStorage 中

```javascript
  computed: {
    checkChage() {
      // 使用 array.map() 遍歷陣列中所有的指定元素，並組成一個新陣列
      return this.eventList.map(
        eventList =>
          eventList.title +
          eventList.comments +
          eventList.isComplete +
          eventList.isImportant
      );
    }
  },
    watch: {
    checkChage() {
      // 追蹤 checkChage() 函數的 return 值是否有任何變化，若有的話將 eventList 轉成 JSON 格式
      // 並存在 localStorage 中
      localStorage.setItem("eventList", JSON.stringify(this.eventList));
    }
  },
```

### 9-2. 載入資料

載入資料就非常簡單了，我們直接在 mounted() 中加入判斷，如果 localStorage 中有我們的資料，就抓出來並賦值到 eventList 上

```javascript
  mounted() {
    // 如果有 localStorage(eventList) 就賦值到 this.eventList 中
    if (localStorage.getItem("eventList")) {
      this.eventList = JSON.parse(localStorage.getItem("eventList") || "[]");
    }
```

P.S. 為甚麽是在 mounted() 中加入判斷？這邊要看一下 Vue 的生命週期圖，mounted() 是在 template 渲染後的入口點，基本上要在 created() 也是可以的
![](https://cn.vuejs.org/images/lifecycle.png)

## 10. 總結

雖然這個 To-Do-List 寫的不是很好，但也從中學習到了很多，果然寫項目對新手來說是不錯的學習的方式 XD，繼續學習....（ 前端的世界好深～～～
