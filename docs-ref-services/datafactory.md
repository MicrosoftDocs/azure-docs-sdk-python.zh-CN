---
title: 用于 Python 的 Azure 数据工厂库
description: 用于 Python 的 Azure 数据工厂库参考
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 05/10/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 05d93f7d1838c110c3ba77f7abd3967f7870774b
ms.sourcegitcommit: d65549030a0edb50d75482f79aac0962d1dacb57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34759042"
---
# <a name="azure-data-factory-libraries-for-python"></a><span data-ttu-id="27c3e-103">用于 Python 的 Azure 数据工厂库</span><span class="sxs-lookup"><span data-stu-id="27c3e-103">Azure Data Factory libraries for Python</span></span>

<span data-ttu-id="27c3e-104">使用 [Azure 数据工厂](/azure/data-factory/)将数据存储、移动和处理服务组成自动的数据管道</span><span class="sxs-lookup"><span data-stu-id="27c3e-104">Compose data storage, movement, and processing services into automated data pipelines with [Azure Data Factory](/azure/data-factory/)</span></span>

<span data-ttu-id="27c3e-105">[详细了解](/azure/data-factory/introduction)数据工厂，并通过[使用 Python 创建数据工厂和管道快速入门](/azure/data-factory/quickstart-create-data-factory-python)来入门。</span><span class="sxs-lookup"><span data-stu-id="27c3e-105">[Learn more](/azure/data-factory/introduction) about Data Factory and get started with the [Create a data factory and pipeline using Python quickstart](/azure/data-factory/quickstart-create-data-factory-python).</span></span> 

## <a name="management-module"></a><span data-ttu-id="27c3e-106">管理模块</span><span class="sxs-lookup"><span data-stu-id="27c3e-106">Management module</span></span>

<span data-ttu-id="27c3e-107">使用管理模块在订阅中创建和管理数据工厂实例。</span><span class="sxs-lookup"><span data-stu-id="27c3e-107">Create and manage Data Factory instances in your subscription with the management module.</span></span>

### <a name="installation"></a><span data-ttu-id="27c3e-108">安装</span><span class="sxs-lookup"><span data-stu-id="27c3e-108">Installation</span></span>

<span data-ttu-id="27c3e-109">使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 来安装包：</span><span class="sxs-lookup"><span data-stu-id="27c3e-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a><span data-ttu-id="27c3e-110">示例</span><span class="sxs-lookup"><span data-stu-id="27c3e-110">Example</span></span> 

<span data-ttu-id="27c3e-111">在“美国东部”区域的订阅中创建数据工厂。</span><span class="sxs-lookup"><span data-stu-id="27c3e-111">Create a Data Factory in your subscription on the East US region.</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.datafactory import DataFactoryManagementClient
from azure.mgmt.datafactory.models import *
import time

#Create a data factory
subscription_id = '<Specify your Azure Subscription ID>'
credentials = ServicePrincipalCredentials(client_id='<Active Directory application/client ID>', secret='<client secret>', tenant='<Active Directory tenant ID>')
adf_client = DataFactoryManagementClient(credentials, subscription_id)

rg_params = {'location':'eastus'}
df_params = {'location':'eastus'}  

df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="27c3e-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="27c3e-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datafactory/management)