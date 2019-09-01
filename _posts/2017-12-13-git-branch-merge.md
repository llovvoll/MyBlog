---
layout: single
title: "Git Merge Branch"
date: 2017-12-13 16:37:00 +08:00
modified: 2018-01-03 22:15:24 +08:00
description:
categories:
    - "Tech Article"
tags:
    - Git
header:
---

我平常開發的專案很少切分支來操作，所以常常碰到有分支的專案需要合併提交時，就常常搞失憶怎麼做，這邊就做個筆記一下，以防又忘了~

Branch-A 為主分支，Branch-B 為副分支，情況模擬為已經在副分支完成提交(Git Commit)動作，接下來即可按下列步驟合併至主分支

```
Git Checkout Branch-A (切換至主分支)
Git Pull (獲取最新修改)
Git Checkout Branch-B (切換至副分支)
Git Rebase Branch-A (合併主分支修改)
Git Checkout Branch-A (切換至主分支)
Git Merge Branch-B (合併副分支的修改)
Git Push (提交遠程主分支)
```