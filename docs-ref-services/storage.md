---
title: 用于 Python 的 Azure 存储库
description: ''
keywords: Azure, Python, SDK, API, 存储
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: e45b12af9e026e0f6390556813385d86784feaa4
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2018
ms.locfileid: "35720058"
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="22bfa-103">用于 Python 的 Azure 存储库</span><span class="sxs-lookup"><span data-stu-id="22bfa-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="22bfa-104">概述</span><span class="sxs-lookup"><span data-stu-id="22bfa-104">Overview</span></span>
- <span data-ttu-id="22bfa-105">通过 [Azure Blob 存储](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)读取和写入对象与文件</span><span class="sxs-lookup"><span data-stu-id="22bfa-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="22bfa-106">使用 [Azure 队列存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)在已连接到云的应用程序之间发送和接收消息</span><span class="sxs-lookup"><span data-stu-id="22bfa-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="22bfa-107">使用 [Azure 表存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)读取和写入大型结构化数据</span><span class="sxs-lookup"><span data-stu-id="22bfa-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="22bfa-108">使用 [Azure 文件存储](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)在应用之间共享存储</span><span class="sxs-lookup"><span data-stu-id="22bfa-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="22bfa-109">使用管理库通过 Python 代码创建、更新和管理 Azure 存储帐户，以及查询和重新生成访问密钥。</span><span class="sxs-lookup"><span data-stu-id="22bfa-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="22bfa-110">安装库</span><span class="sxs-lookup"><span data-stu-id="22bfa-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="22bfa-111">Client</span><span class="sxs-lookup"><span data-stu-id="22bfa-111">Client</span></span>

<span data-ttu-id="22bfa-112">Azure 存储客户端库包含 4 个包：Blob、文件、队列和表。</span><span class="sxs-lookup"><span data-stu-id="22bfa-112">Azure Storage Client Libraries consist of 4 packages: Blob, File, Queue and Table.</span></span> <span data-ttu-id="22bfa-113">若要安装 Blob 包，请运行：</span><span class="sxs-lookup"><span data-stu-id="22bfa-113">To install the blob package, run:</span></span>

```bash
pip install azure-storage-blob
```

### <a name="management"></a><span data-ttu-id="22bfa-114">管理</span><span class="sxs-lookup"><span data-stu-id="22bfa-114">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="22bfa-115">示例</span><span class="sxs-lookup"><span data-stu-id="22bfa-115">Example</span></span>
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

## <a name="samples"></a><span data-ttu-id="22bfa-116">示例</span><span class="sxs-lookup"><span data-stu-id="22bfa-116">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="22bfa-117">在 Python 中开始使用 Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="22bfa-117">Get started with Azure Blob Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | <span data-ttu-id="22bfa-118">在 Azure 存储中创建、读取、更新、限制访问和删除文件与对象。</span><span class="sxs-lookup"><span data-stu-id="22bfa-118">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="22bfa-119">在 Python 中开始使用 Azure 队列存储</span><span class="sxs-lookup"><span data-stu-id="22bfa-119">Get started with Azure Queue Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | <span data-ttu-id="22bfa-120">在 Azure 存储队列中插入、扫视、检索和删除消息。</span><span class="sxs-lookup"><span data-stu-id="22bfa-120">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="22bfa-121">管理 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="22bfa-121">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="22bfa-122">创建、更新和删除存储帐户。</span><span class="sxs-lookup"><span data-stu-id="22bfa-122">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="22bfa-123">检索和重新生成存储帐户访问密钥。</span><span class="sxs-lookup"><span data-stu-id="22bfa-123">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="22bfa-124">详细了解可在应用中使用的[示例 Python 代码](https://azure.microsoft.com/resources/samples/?platform=python)。</span><span class="sxs-lookup"><span data-stu-id="22bfa-124">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>
