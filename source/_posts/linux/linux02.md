---
title: Copy File or Directory to Linux VM Cloud 
date: 2021-07-13 10:38:04
categories: Linux
description: Copy File to Linux VM Cloud 
tags: 
  - Linux
---

要在不同的 Linux 主機之間複製檔案，可透過 scp 指令，它可以透過 SSH 安全加密傳輸的方式，將本地端的檔案或目錄複製到遠端，或是將遠端的資料複製到本地端，而這個指令在 Mac OS X 中也同樣可以使用。

### 複製檔案
``` linux
scp [帳號@來源主機]:來源檔案 [帳號@目的主機]:目的檔案

# 從本地端複製到遠端
scp /path/file1 myuser@ip位址:/path/file2

# 從遠端複製到本地端
scp myuser@ip位址:/path/file2 /path/file1

```
這裡的帳號就是登入主機上的帳號（類似ssh指令的用法），如果省略帳號與主機，只寫一般的檔案路徑的話，就是代表本機的檔案。

### 複製目錄
若要複製整個目錄以及其下的所有檔案，則加上 `-r` 參數，若有金鑰則使用 `-i` 參數並帶入金鑰位置 ：
``` linux
scp -i [.pem key position] -r [local directory path] azureuser@ip_address:[remote directory path]

指令架構是
scp -i /金鑰位置 -r /本地位置 遠端位置
遠端位置是 username@ip:/remote/file/path
```

### 資料壓縮

若要將資料壓縮之後再傳送，減少網路頻寬的使用量，可以加上 -C 參數：
``` linux
# 資料壓縮
scp -C /path/file1 myuser@ip位址:/path/file2
```

### 限制傳輸速度

若要限制網路的使用頻寬，可以用 -l 指定可用的網路頻寬上限值（單位為 Kbit/s）：
``` linux
# 限制傳輸速度為 400 Kbit/s
scp -l 400 /path/file1 myuser@ip位址:/path/file2
```

### 自訂連接埠

一般 SSH 伺服器的連接埠號為 22，如果遇到使用非標準埠號的伺服器，可以用 -P 來指定埠號。若遠端的 SSH 伺服器使用 2222 這個連接埠，我們就可以這樣複製檔案：
``` linux
# 使用 2222 連接埠
scp -P 2222 /path/file1 myuser@ip位址:/path/file2
```