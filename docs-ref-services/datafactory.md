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
# <a name="azure-data-factory-libraries-for-python"></a>用于 Python 的 Azure 数据工厂库

使用 [Azure 数据工厂](/azure/data-factory/)将数据存储、移动和处理服务组成自动的数据管道

[详细了解](/azure/data-factory/introduction)数据工厂，并通过[使用 Python 创建数据工厂和管道快速入门](/azure/data-factory/quickstart-create-data-factory-python)来入门。 

## <a name="management-module"></a>管理模块

使用管理模块在订阅中创建和管理数据工厂实例。

### <a name="installation"></a>安装

使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 来安装包：

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a>示例 

在“美国东部”区域的订阅中创建数据工厂。

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
> [了解管理 API](/python/api/overview/azure/datafactory/management)