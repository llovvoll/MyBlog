---
layout: single
title: "Git ignore .DS_Store & Atom Hide .DS_Store"
date: 2018-08-21
modified:
description:
categories:
    - "Tech Article"
tags:
    - Git
    - Atom
header:
---

相信不少人都曾經不小心把.DS_Store給Push上去的經驗吧，一不小心就Push了一大堆且在每個資料夾中，之後還得花時間刪除掉在Push一次，非常的麻煩，今天就來分享一下如何設定Git無視.DS_Store及Atom不顯示.DS_Store。

## Git ignore .DS_Store

Steps 1: Create .gitignore_global & Editing .gitignore_global
```console
$ cd ~
$ touch .gitignore_global
$ vi .gitignore_global

.DS_Store

*/.DS_Store
```

Steps 2: Editing .gitconfig
```console
$ cd ~
$ vi .gitconfig

[core]

excludesfile = /Users/<UserName>/.gitignore_global
```

Steps 3: Restart Terminal

## Atom Hide .DS_Store

Steps 1: Atom > Prefernces

Steps 2: Packages > tree-view > settings

Steps 3: Enable Hide Ignored Names & Hide VCS ignored Files

打完收工 ヽ(́◕◞౪◟◕‵)ﾉ
