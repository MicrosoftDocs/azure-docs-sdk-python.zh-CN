---
title: 用于 Python 的 Azure 存储库
description: ''
keywords: Azure, Python, SDK, API, 存储
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 1cafbe5558c49e238182b7efabeae6fcb96d43d8
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534197"
---
# <a name="azure-storage-libraries-for-python"></a>用于 Python 的 Azure 存储库

## <a name="overview"></a>概述
- 通过 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)读取和写入对象与文件
- 使用 [Azure 队列存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)在已连接到云的应用程序之间发送和接收消息
- 使用 [Azure 表存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)读取和写入大型结构化数据 
- 使用 [Azure 文件存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)在应用之间共享存储

使用管理库通过 Python 代码创建、更新和管理 Azure 存储帐户，以及查询和重新生成访问密钥。

## <a name="install-the-libraries"></a>安装库

### <a name="client"></a>Client

Azure 存储客户端库包含 4 个包：Blob、文件、队列和表。 若要安装 Blob 包，请运行：

```bash
pip install azure-storage-blob
```

### <a name="management"></a>管理

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>示例
```python
from azure.storage.blob import BlockBlobService

blob_service = BlockBlobService(account_name, account_key)

blob_service.create_container(
    'mycontainername',
    public_access=PublicAccess.Blob
)

blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url('mycontainername', 'myblobname'))
```

## <a name="samples"></a>示例

| | |
|--|--|
| [在 Python 中开始使用 Azure Blob 存储](https://docs.microsoft.com/azure/storage/blobs/storage-python-how-to-use-blob-storage) | 在 Azure 存储中创建、读取、更新、限制访问和删除文件与对象。 |
| [在 Python 中开始使用 Azure 队列存储](https://docs.microsoft.com/azure/storage/queues/storage-python-how-to-use-queue-storage) | 在 Azure 存储队列中插入、扫视、检索和删除消息。 | 
| [管理 Azure 存储帐户](https://azure.microsoft.com/resources/samples/storage-python-manage) | 创建、更新和删除存储帐户。 检索和重新生成存储帐户访问密钥。

详细了解可在应用中使用的[示例 Python 代码](https://azure.microsoft.com/resources/samples/?platform=python)。
