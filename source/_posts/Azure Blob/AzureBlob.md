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

### 請遵循下列步驟來取得 Blob 服務 SAS(Shared access signatures) URL：

在 Azure 入口網站中，選取您的儲存體帳戶。
* 瀏覽至 [設定] 區段，然後選取 [共用存取簽章]。
* 向下捲動，然後按一下 [產生 SAS 與連接字串] 按鈕。
* 進一步向下捲動並找出 [Blob 服務 SAS URL] 欄位。
* 按一下 [Blob 服務 SAS URL] 欄位最右側的 [複製到剪貼簿] 按鈕。
* 將複製的 URL 儲存在某處，以便在後續的步驟中使用。

## Get blob from store account, transfer to data URL


``` js
// 引用 @azure/storage-blob套件
const { BlobServiceClient } = require("@azure/storage-blob");
// Blob service SAS URL (要先去Azure services 設定)
// srt = SignedResourceTypes (對應到 Azure Portal Allowed Resource Type : Service, Container, Object)
// ss = SignedServices(對應到 Azure Portal Allowed Services : Blob, File, Quere, Table)
// sp = SignedPermission(對應到 Azure Portal Allowed Permissions : Read, Write, Delete ...)
// sig = Base64 encoded signature generated using by the HMAC-SHA256 algorithm.
const blobSasUrl = "https://{{Azure_storage_account}}.blob.core.windows.net/?sv=2020-02-10&ss=bfqt&srt=sco&sp=rwdlacuptfx&se=2021-10-30T13:47:50Z&st=2021-07-05T05:47:50Z&spr=https&sig=wWqlPt7BA6uYAdEmsv05DbAlyhV7qJUwmTPLPx14NSU%3D";
// 設定完這些參數需要透過Store Account Key 去把 signature 簽出來

// Azure Storage supports several ways to authenticate. 
// In order to interact with the Azure Blob Storage service you'll need to create an instance of a Storage client 
// - BlobServiceClient, ContainerClient, or BlobClient
const blobServiceClient = new BlobServiceClient(blobSasUrl);
// Container Name in your BlobServiceClient
const containerName = '0375fdf7b1ce594d'  // Azure Storage Accout 內的container name
const containerClient = blobServiceClient.getContainerClient(containerName);
const blobClient = containerClient.getBlobClient('2020-07-01--17-34-44/2020-07-01--17-34-44--0/qcamera.m3u8'); // blob 資料存放路徑

export async function downLoad() {
  const downloadBlockBlobResponse = await blobClient.download();
  return blobToString(await downloadBlockBlobResponse.blobBody);
}
// blob 資料轉成 data URL
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

成功將blob資料轉換成 Data URL 後，因為不可能每次產SAS都進後台去產，所以研究了如何自行產出SAS。
在公司專案中，不同用戶的資料會存在同一個 Storage account 但不同的 containers ，因此本篇只針對如何產出某個特定 container 的SAS。 


## Node.js 用 generateBlobSASQueryParameters 產 SAS token

``` js
// generateBlobSASQueryParameters Generate service level SAS for a container
const AzureStorageBlob = require('@azure/storage-blob')
// Enter your storage account name and shared key
const account = `${store account name}`;
const accountKey = `${store account shared key}`;
const sharedKeyCredential = new AzureStorageBlob.StorageSharedKeyCredential(account, accountKey)
const containerName = `${store account 下的 container name}`
// SAS 的有效期限
const last_day_of_this_year = new Date(new Date().getFullYear(), 11, 31)
const startOn = new Date()
// 產出針對某個container 的SAS，下方generateBlobSASQueryParameters第一個object 參數的內容跟產出ss, srt, sp ... 有關下方參考資料有提供
const containerSAS = AzureStorageBlob.generateBlobSASQueryParameters({
  containerName, // Required
  permissions: AzureStorageBlob.ContainerSASPermissions.parse("r"), // Required
  startsOn:startOn,
  expiresOn:last_day_of_this_year,
  protocol: AzureStorageBlob.SASProtocol.Https, // Optional
},
sharedKeyCredential
).toString();
```

## python generate_container_sas 產 SAS token

``` python
# generate container sas token 
from azure.storage.blob import BlobSasPermissions
from azure.storage.blob import generate_container_sas
from datetime import timedelta, datetime
import os
# generate SAS token for a container (fastapi)
@app.get("/getSAS")
def getSAS(request: Request, q: Optional[str] = None):
    sas_account_name = os.getenv('ACCOUNT_NAME')
    sas_account_key = os.getenv('ACCOUNT_KEY')
    container_name = f"{store container name}"
    sas_container = generate_container_sas(
        sas_account_name,  # type: str
        container_name,  # type: str
        account_key=sas_account_key,  # type: Optional[str]
        permission=BlobSasPermissions(read=True),
        expiry=datetime.utcnow() + timedelta(hours=2)
    )
    return JSONResponse(content={"sastoken": sas_container})
```

### 參考資料
* [快速入門：在瀏覽器中使用 JavaScript v12 SDK 來管理 Blob](https://docs.microsoft.com/zh-tw/azure/storage/blobs/quickstart-blobs-javascript-browser)

* [How to Use SAS Tokens with Azure Blob Storage](https://nxt.engineering/en/blog/sas_token/)

* [Azure document for @azure/storage-blob](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-storage-blob/12.6.0/index.)

* [Create an account SAS](https://docs.microsoft.com/en-us/rest/api/storageservices/create-account-sas)

* [SAS 格式參考](https://docs.microsoft.com/en-us/javascript/api/@azure/storage-blob/accountsassignaturevalues?view=azure-node-latest#permissions)

* [Generate service level SAS for a container](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-storage-blob/12.6.0/globals.html#generateblobsasqueryparameters)

* [Azure SDK for Python](https://docs.microsoft.com/en-us/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient?view=azure-python)

* [Python Generate SAS](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-blob/12.0.0b5/azure.storage.blob.html?highlight=generate_container_sas#azure.storage.blob.generate_container_sas)

* [Python Generate SAS Demo](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L145)

* [Azure SDK for JavaScript](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-storage-blob/12.6.0/index.html)