---
title: Azure SDK Upload File or Folder To Azure Storage Account
date: 2021-07-22 10:01:13
description: Upload File or Folder To Azure Storage Account with azure-storage-blob
categories: Azure Storage
tags:
  - Azure Storage
  - Python 
  - Python azure-storage-blob
---

### 上傳單一檔案
實作前，要先去 Azure Storage Account 創建一個 Storage Account，並 pip install azure-storage-blob。

``` python
import sys
import requests
import os
from azure.storage.blob import BlobClient, BlobServiceClient
# Storage Account 上的 Access key & Connection string
accoubt_key = os.getenv('ACCOUNT_KEY')
connection_string = os.getenv('CONNECTION_STRING')
# 取得 blob storage 的 service client
service_client = BlobServiceClient.from_connection_string(connection_string, accoubt_key, logging_enable=True)
container_name = 'test'
# 取得 container client, 名稱為 test
container = service_client.get_container_client(container_name)
# 檢查container 是否存在，若不存在則創建container
if not container.exists():
    container = service_client.create_container(container_name, public_access='container')

# 開始上傳檔案
print("*****upload file*****")
# 要上傳的檔案路徑
upload_file_path="./qcamera.ts"
# 帶有sastoken的url
# storage_account_name : 要到 Storage Account 查看名稱
# /test/routename/segmentname : 在Storage Account上存 qcamera.ts 的路徑，test就是container，之後的routename/segmentname 就是存 blob 的路徑
# sastoken 可以針對 BlobServiceClient 生產，或 ContainerService 生產
sas_url=f"https://{storage_account_name}.blob.core.windows.net/test/routename/segmentname/qcamera.ts?{sastoken}"
# 透過 sas url 取得 blob client
client = BlobClient.from_blob_url(sas_url)
# 開啟檔案二元碼
with open(upload_file_path,'rb') as data:
   try:
      # 透過 blob client 上傳開啟的檔案
       client.upload_blob(data)
   except:
      print("error:"+ str(sys.exc_info()[0]))
       ex_type, ex_val, ex_stack = sys.exc_info()
       print(ex_type)
       print(ex_val)
```

### 上傳一個資料夾內的資料

跟上完單一檔案一樣的方式，透過迴圈開啟每個檔案的二元碼並上傳。
``` python
print(" *****upload folder***** ")
account_url= "https://{storage_account_name}.blob.core.windows.net"
# 取得 blob storage 的 service client
blob_service_client = BlobServiceClient(account_url, sastoken)
container_name = "test"
# 取得 container client
container_client = blob_service_client.get_container_client(container_name)
local_path="./test"
foldername="test"
# 透過迴圈逐一開啟檔案二元碼
for files in os.listdir(local_path):
   with open(os.path.join(local_path,files),"rb") as data:
      #  透過 blob service client 取得 blob client
      #  並逐一將檔案上傳到 blob client 的路徑
       blob_client = blob_service_client.get_blob_client(container=container_name, blob=foldername +"/" + files)
       blob_client.upload_blob(data)
```