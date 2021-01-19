---
layout: single
title: "Mac 使用 FreeTDS 搭配 Pyodbc 連接 SQL Server 2000"
date: 2021-01-19
modified:
description: "Mac 使用 FreeTDS 搭配 Pyodbc 連接 SQL Server 2000"
categories:
  - "Tech Article"
tags:
  - Python
---

最近手上有一個案子必需連到客戶的舊有 ERP 資料庫上去撈一些資料回來處理，不得不說甚麼時代了還在用 SQL Server 2000，一開始為了處理連線問題真的耗費不少腦細胞，一直想用 ODBC Driver for SQL Server 來連線，但遇到一堆問題，後來才發現 [FreeTDS](https://www.freetds.org/) 來使用方便快速多了

# Install Unixodbc & Freetds

```
$ brew install unixodbc freetds
$ odbcinst -j
unixODBC 2.3.9
DRIVERS............: /usr/local/etc/odbcinst.ini
SYSTEM DATA SOURCES: /usr/local/etc/odbc.ini
FILE DATA SOURCES..: /usr/local/etc/ODBCDataSources
USER DATA SOURCES..: /Users/rick/.odbc.ini
SQLULEN Size.......: 8
SQLLEN Size........: 8
SQLSETPOSIROW Size.: 8

$ vi /usr/local/etc/odbcinst.ini
[FreeTDS]
Description = TD Driver (MSSQL)
Driver = /usr/local/lib/libtdsodbc.so
Setup = /usr/local/lib/libtdsodbc.so
FileUsage = 1
```

# Pyodbc Connection

```
import pyodbc

conn = pyodbc.connect(
    'DRIVER={FreeTDS};SERVER=ovvo.cc,1433;DATABASE=test;UID=test;PWD=test')
cursor = conn.cursor()
```
