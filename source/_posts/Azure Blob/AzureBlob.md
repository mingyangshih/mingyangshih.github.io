---
title: JavaScript v12 SDK Manage Blob
date: 2021-07-05
description: 透過Browser取得存在Azure Storage內的 Blob data
categories: Azure Storage
tags: 
  - Azure Storage
  - Frontend
  - React
---

因工作上須透過Browser取得存在Azure Storage內的 Blob data，在此做個紀錄

## Blob 儲存體提供三種類型資源：

* 儲存體帳戶 BlobServiceClient : 此類別可讓您操作 Azure 儲存體資源和 Blob 容器。
* 儲存體帳戶中的容器 ContainerClient : 此類別可讓您操作 Azure 儲存體容器及其 Blob。
* 容器中的 Blob BlobClient : 類別可讓您操作 Azure 儲存體 Blob。

## 建立 CORS 規則
您必須先設定帳戶以啟用跨原始資源共用 (或簡稱為 CORS)，Web 應用程式才可從用戶端存取 Blob 儲存體。

## 建立共用存取簽章

在瀏覽器中執行的程式碼會使用共用存取簽章 (SAS) 來授權 Azure Blob 儲存體要求。 藉由使用 SAS，用戶端不需要帳戶存取金鑰或連接字串，即可為儲存體資源的存取授權。 如需 SAS 的詳細資訊，請參閱使用共用存取簽章 (SAS)。

### 請遵循下列步驟來取得 Blob 服務 SAS URL：

在 Azure 入口網站中，選取您的儲存體帳戶。
* 瀏覽至 [設定] 區段，然後選取 [共用存取簽章]。
* 向下捲動，然後按一下 [產生 SAS 與連接字串] 按鈕。
* 進一步向下捲動並找出 [Blob 服務 SAS URL] 欄位。
* 按一下 [Blob 服務 SAS URL] 欄位最右側的 [複製到剪貼簿] 按鈕。
* 將複製的 URL 儲存在某處，以便在後續的步驟中使用。

## 新增 Azure Blob 儲存體用戶端程式庫
[Azure SDK for Python](https://docs.microsoft.com/en-us/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient?view=azure-python)

[Azure SDK for JavaScript](https://azure.github.io/azure-sdk-for-js/index.html)

``` js
// 引用 @azure/storage-blob套件
const { BlobServiceClient } = require("@azure/storage-blob");
// Blob service SAS URL (要先去Azure services 設定)
const blobSasUrl = "https://{{Azure_storage_account}}.blob.core.windows.net/?sv=2020-02-10&ss=bfqt&srt=sco&sp=rwdlacuptfx&se=2021-10-30T13:47:50Z&st=2021-07-05T05:47:50Z&spr=https&sig=wWqlPt7BA6uYAdEmsv05DbAlyhV7qJUwmTPLPx14NSU%3D";
// Create a new BlobServiceClient
const blobServiceClient = new BlobServiceClient(blobSasUrl);
// Container Name in your BlobServiceClient
const containerName = '0375fdf7b1ce594d'  // Azure Storage Accout 內的container name
const containerClient = blobServiceClient.getContainerClient(containerName);
const blobClient = containerClient.getBlobClient('2020-07-01--17-34-44/2020-07-01--17-34-44--0/qcamera.m3u8'); // blob 資料存放路徑

export async function downLoad() {
  const downloadBlockBlobResponse = await blobClient.download();
  return blobToString(await downloadBlockBlobResponse.blobBody);
}

function blobToString(blob) {
  const fileReader = new FileReader();
  return new Promise((resolve, reject) => {
    fileReader.onloadend = (ev) => {
      resolve(ev.target.result);
    };
    fileReader.onerror = reject;
    fileReader.readAsDataURL(blob);
  });
}
```

### 參考資料
https://docs.microsoft.com/zh-tw/azure/storage/blobs/quickstart-blobs-javascript-browser
https://nxt.engineering/en/blog/sas_token/