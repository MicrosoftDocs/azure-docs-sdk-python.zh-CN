---
title: 用于 Python 的 Azure Batch 库
description: Python Batch 库的参考文档
keywords: Azure, Python, SDK, API, Batch, 处理, 计划, 长时间运行
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: de5f3a98b1712ff9bdcc417daf10719178819364
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478980"
---
# <a name="azure-batch-libraries-for-python"></a><span data-ttu-id="aacfa-104">用于 Python 的 Azure Batch 库</span><span class="sxs-lookup"><span data-stu-id="aacfa-104">Azure Batch libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="aacfa-105">概述</span><span class="sxs-lookup"><span data-stu-id="aacfa-105">Overview</span></span>

<span data-ttu-id="aacfa-106">使用 [Azure Batch](/azure/batch/batch-technical-overview) 在云中有效运行大规模并行和高性能计算应用程序。</span><span class="sxs-lookup"><span data-stu-id="aacfa-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>   

<span data-ttu-id="aacfa-107">若要开始使用 Azure Batch，请参阅[使用 Azure 门户创建 Batch 帐户](/azure/batch/batch-account-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="aacfa-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="aacfa-108">安装库</span><span class="sxs-lookup"><span data-stu-id="aacfa-108">Install the libraries</span></span>

## <a name="client-library"></a><span data-ttu-id="aacfa-109">客户端库</span><span class="sxs-lookup"><span data-stu-id="aacfa-109">Client library</span></span>
<span data-ttu-id="aacfa-110">使用 Azure Batch 客户端库可以配置计算节点和池、定义任务并将其配置为在作业中运行，以及设置一个作业管理器来控制和监视作业执行。</span><span class="sxs-lookup"><span data-stu-id="aacfa-110">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="aacfa-111">[详细了解](/azure/batch/batch-api-basics)如何使用这些对象来运行大规模并行计算解决方案。</span><span class="sxs-lookup"><span data-stu-id="aacfa-111">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

```bash
pip install azure-batch
```
### <a name="example"></a><span data-ttu-id="aacfa-112">示例</span><span class="sxs-lookup"><span data-stu-id="aacfa-112">Example</span></span>

<span data-ttu-id="aacfa-113">在 Batch 帐户中设置 Linux 计算节点池：</span><span class="sxs-lookup"><span data-stu-id="aacfa-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```python
# create the batch client for an account using its URI and keys
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

## <a name="management-api"></a><span data-ttu-id="aacfa-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="aacfa-114">Management API</span></span>
<span data-ttu-id="aacfa-115">使用 Azure Batch 管理库可以创建和删除 Batch 帐户、读取和重新生成 Batch 帐户密钥，以及管理 Batch 帐户存储。</span><span class="sxs-lookup"><span data-stu-id="aacfa-115">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="aacfa-116">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="aacfa-116">Explore the Client APIs</span></span>](/python/api/overview/azure/batch/client)

### <a name="example"></a><span data-ttu-id="aacfa-117">示例</span><span class="sxs-lookup"><span data-stu-id="aacfa-117">Example</span></span>
<span data-ttu-id="aacfa-118">创建 Azure Batch 帐户并为其配置新的应用程序和 Azure 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="aacfa-118">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```python
from azure.mgmt.batch import BatchManagementClient
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

LOCATION ='eastus'
GROUP_NAME ='batchresourcegroup'
STORAGE_ACCOUNT_NAME ='batchstorageaccount'

# Create Resource group
print('Create Resource Group')
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})

# Create a storage account
storage_async_operation = storage_client.storage_accounts.create(
    GROUP_NAME,
    STORAGE_ACCOUNT_NAME,
    StorageAccountCreateParameters(
        sku=Sku(SkuName.standard_ragrs),
        kind=Kind.storage,
        location=LOCATION
    )
)
storage_account = storage_async_operation.result()

# Create a Batch Account, specifying the storage account we want to link
storage_resource = storage_account.id
batch_account = azure.mgmt.batch.models.BatchAccountCreateParameters(
    location=LOCATION,
    auto_storage=azure.mgmt.batch.models.AutoStorageBaseProperties(storage_resource)
)
creating = batch_client.account.create('MyBatchAccount', LOCATION, batch_account)
creating.wait()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="aacfa-119">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="aacfa-119">Explore the Management APIs</span></span>](/python/api/overview/azure/batch/management)