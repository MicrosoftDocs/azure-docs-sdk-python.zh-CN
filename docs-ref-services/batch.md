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
# <a name="azure-batch-libraries-for-python"></a>用于 Python 的 Azure Batch 库

## <a name="overview"></a>概述

使用 [Azure Batch](/azure/batch/batch-technical-overview) 在云中有效运行大规模并行和高性能计算应用程序。   

若要开始使用 Azure Batch，请参阅[使用 Azure 门户创建 Batch 帐户](/azure/batch/batch-account-create-portal)。

## <a name="install-the-libraries"></a>安装库

## <a name="client-library"></a>客户端库
使用 Azure Batch 客户端库可以配置计算节点和池、定义任务并将其配置为在作业中运行，以及设置一个作业管理器来控制和监视作业执行。 [详细了解](/azure/batch/batch-api-basics)如何使用这些对象来运行大规模并行计算解决方案。

```bash
pip install azure-batch
```
### <a name="example"></a>示例

在 Batch 帐户中设置 Linux 计算节点池：

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

## <a name="management-api"></a>管理 API
使用 Azure Batch 管理库可以创建和删除 Batch 帐户、读取和重新生成 Batch 帐户密钥，以及管理 Batch 帐户存储。

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [了解客户端 API](/python/api/overview/azure/batch/client)

### <a name="example"></a>示例
创建 Azure Batch 帐户并为其配置新的应用程序和 Azure 存储帐户。

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
> [了解管理 API](/python/api/overview/azure/batch/management)