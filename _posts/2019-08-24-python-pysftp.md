---
layout: single
title: "Python 使用PySftp連線SFTP Server"
date: 2019-08-24
modified:
description:
categories:
    - "Tech Article"
tags:
    - Python
    - PySftp
header:
---

這週收到印度分公司的消息，因爲法規的因素需要總公司將印度JDE的資料庫備份到印度Amazon S3上，一開始DBA的做法是每個月手動上傳，主管知道後發現每個月手動上傳的方式可能會有遺漏的可能，所以詢問了是否有自動化的可能性，所以我就接下這個任務來達成自動化

<!-- Table of Contents -->
{% include toc icon="heart" title="ToC" %}

這次選擇使用Python + [PySftp](https://pysftp.readthedocs.io/en/release_0.2.9/)來達成目的，這邊記錄一下PySftp的基礎使用方式及利用Docker建立一個SFTP Server容器來方便進行測試

## 安裝 PySftp
```sh
pip install pysftp
```

## 建立 Docker SFTP Server 容器
```sh
# UserName = sftp
# PassWord = sftp

docker run -p 22:22 -d atmoz/sftp sftp:sftp:::test
```

## 官方範例
```python
import pysftp

with pysftp.Connection('hostname', username='me', password='secret') as sftp:
    with sftp.cd('public'):             # temporarily chdir to public
        sftp.put('/my/local/filename')  # upload file to public/ on remote
        sftp.get('remote_file')         # get a remote file
```

這個範例雖然可以正常運行，但是會出現下方的錯誤訊息，因爲當連線SFTP Server時會檢查HostKey，如果沒有導入的話則會出現下方的錯誤訊息

```sh
Traceback (most recent call last):
  File "1.py", line 3, in <module>
    with pysftp.Connection('localhost', username='sftp', password='sftp') as sftp:
  File "/usr/local/lib/python3.7/site-packages/pysftp/__init__.py", line 132, in __init__
    self._tconnect['hostkey'] = self._cnopts.get_hostkey(host)
  File "/usr/local/lib/python3.7/site-packages/pysftp/__init__.py", line 71, in get_hostkey
    raise SSHException("No hostkey for host %s found." % host)
paramiko.ssh_exception.SSHException: No hostkey for host localhost found.
Exception ignored in: <function Connection.__del__ at 0x10f2f8cb0>
Traceback (most recent call last):
  File "/usr/local/lib/python3.7/site-packages/pysftp/__init__.py", line 1013, in __del__
    self.close()
  File "/usr/local/lib/python3.7/site-packages/pysftp/__init__.py", line 784, in close
    if self._sftp_live:
AttributeError: 'Connection' object has no attribute '_sftp_live'
```

所以當我們測試時，我們可以建立一個假的HostKey並導入，這樣測試時就不會跳出錯誤訊息了，但是當正式環境時建議還是使用正確的HostKey確保連線安全

```sh
# ./known_hosts
127.0.0.1 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBUATNpl42C0DIihcIp1IolH+X8e/qHe3PEM6uh0qPdXJmjaH+RMjb2ldh5dAl7l7/a6LPecHHHWOwDYhmRv68M=
```

## 列出目錄檔案
```python
import pysftp

sHostName = 'localhost'
sUserName = 'sftp'
sPassWord = 'sftp'

cnopts = pysftp.CnOpts(knownhosts='known_hosts')
cnopts.hostkeys = None

with pysftp.Connection(sHostName, username=sUserName, password=sPassWord, cnopts=cnopts) as sftp:
    
    # 移動目錄
    sftp.cwd('./test/')
    
    # 取得目錄內容
    directory = sftp.listdir_attr()

    # 印出結果
    for attr in directory:
        print (attr.filename, attr)
```

### 檔案上傳
```
import pysftp

sHostName = 'localhost'
sUserName = 'sftp'
sPassWord = 'sftp'

cnopts = pysftp.CnOpts(knownhosts='known_hosts')
cnopts.hostkeys = None

with pysftp.Connection(sHostName, username=sUserName, password=sPassWord, cnopts=cnopts) as sftp:
    
    # 移動目錄
    sftp.cwd('./test/')

    # 上傳檔案
    sftp.put('/Users/rick/GitHub/1.py')
```

### 檔案下載
```python
import pysftp

sHostName = 'localhost'
sUserName = 'sftp'
sPassWord = 'sftp'

cnopts = pysftp.CnOpts(knownhosts='known_hosts')
cnopts.hostkeys = None

with pysftp.Connection(sHostName, username=sUserName, password=sPassWord, cnopts=cnopts) as sftp:

    # 檔案下載 sftp.get('遠端檔案位置', '本機檔案位置')
    sftp.get('/test/1.py','./ttt.py')
```

### 檔案刪除
```python
import pysftp

sHostName = 'localhost'
sUserName = 'sftp'
sPassWord = 'sftp'

cnopts = pysftp.CnOpts(knownhosts='known_hosts')
cnopts.hostkeys = None

with pysftp.Connection(sHostName, username=sUserName, password=sPassWord, cnopts=cnopts) as sftp:

    # 檔案刪除 sftp.remove('遠端檔案位置')
    sftp.remove('/test/1.py')
```

以上就是基礎的顯示、上傳、下載、刪除的功能，其它還有許多用法可以參考[PySftp](https://pysftp.readthedocs.io/en/release_0.2.9/)的文件，如果有問題的話，也可以留言一起討論唷