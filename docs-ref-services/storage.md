---
title: "用于 Python 的 Azure 存储库"
description: 
keywords: "Azure, Python, SDK, API, 存储"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 64465964d32934a6a45dec44cb92a0a8a84b9c37
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="c89e7-103">用于 Python 的 Azure 存储库</span><span class="sxs-lookup"><span data-stu-id="c89e7-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="c89e7-104">概述</span><span class="sxs-lookup"><span data-stu-id="c89e7-104">Overview</span></span>
- <span data-ttu-id="c89e7-105">通过 [Azure Blob 存储](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)读取和写入对象与文件</span><span class="sxs-lookup"><span data-stu-id="c89e7-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="c89e7-106">使用 [Azure 队列存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)在已连接到云的应用程序之间发送和接收消息</span><span class="sxs-lookup"><span data-stu-id="c89e7-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="c89e7-107">使用 [Azure 表存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)读取和写入大型结构化数据</span><span class="sxs-lookup"><span data-stu-id="c89e7-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="c89e7-108">使用 [Azure 文件存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)在应用之间共享存储</span><span class="sxs-lookup"><span data-stu-id="c89e7-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="c89e7-109">使用管理库通过 Python 代码创建、更新和管理 Azure 存储帐户，以及查询和重新生成访问密钥。</span><span class="sxs-lookup"><span data-stu-id="c89e7-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="c89e7-110">安装库</span><span class="sxs-lookup"><span data-stu-id="c89e7-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="c89e7-111">客户端</span><span class="sxs-lookup"><span data-stu-id="c89e7-111">Client</span></span>

```bash
pip install azure-storage
```

### <a name="management"></a><span data-ttu-id="c89e7-112">管理</span><span class="sxs-lookup"><span data-stu-id="c89e7-112">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="c89e7-113">示例</span><span class="sxs-lookup"><span data-stu-id="c89e7-113">Example</span></span>
```python
storage_client = CloudStorageAccount(storage_account_name, storage_key)
blob_service = storage_client.create_block_blob_service()

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

## <a name="samples"></a><span data-ttu-id="c89e7-114">示例</span><span class="sxs-lookup"><span data-stu-id="c89e7-114">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="c89e7-115">在 Python 中开始使用 Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="c89e7-115">Get started with Azure Blob Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | <span data-ttu-id="c89e7-116">在 Azure 存储中创建、读取、更新、限制访问和删除文件与对象。</span><span class="sxs-lookup"><span data-stu-id="c89e7-116">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="c89e7-117">在 Python 中开始使用 Azure 队列存储</span><span class="sxs-lookup"><span data-stu-id="c89e7-117">Get started with Azure Queue Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | <span data-ttu-id="c89e7-118">在 Azure 存储队列中插入、扫视、检索和删除消息。</span><span class="sxs-lookup"><span data-stu-id="c89e7-118">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="c89e7-119">管理 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="c89e7-119">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="c89e7-120">创建、更新和删除存储帐户。</span><span class="sxs-lookup"><span data-stu-id="c89e7-120">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="c89e7-121">检索和重新生成存储帐户访问密钥。</span><span class="sxs-lookup"><span data-stu-id="c89e7-121">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="c89e7-122">详细了解可在应用中使用的[示例 Python 代码](https://azure.microsoft.com/resources/samples/?platform=python)。</span><span class="sxs-lookup"><span data-stu-id="c89e7-122">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>