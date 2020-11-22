---
layout: single
title: "Python pip 錯誤 The script is installed in directory, which is not PATH"
date: 2020-11-22
modified:
description: "Python pip 錯誤 The script is installed in directory, which is not PATH"
categories:
  - "Tech Article"
tags:
  - Python
---

pip  安裝 [uvicorn](https://www.uvicorn.org/) 之後會顯示 “The script is installed in directory, which is not PATH” 要啟動時會出現 zsh: command not found: uvicorn

原因是因爲找不到 PATH 導致無法執行，所以只要加上即可

## macOS

```
export PATH="${PATH}:/Users/{User Name}/Library/Python/3.7/bin"
```

如果是使用 zsh 的話要編輯 ~/.zshrc 並加上重開 Terminal 即可解決

```
export PATH="${PATH}:/Users/{User Name}/Library/Python/3.7/bin"
```
