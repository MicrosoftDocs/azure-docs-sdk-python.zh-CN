---
title: 用于 Python 的 Azure 资源库
description: 用于 Python 的 Azure 资源库参考
keywords: Azure, Python, SDK, API, 资源
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: cef1f2bad7dcb3ff73aeae9c56000fb949541df9
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534227"
---
# <a name="azure-resources-libraries-for-python"></a>用于 Python 的 Azure 资源库

## <a name="overview"></a>概述 
使用 [Azure 资源管理器](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)部署、监视和管理组中的资源。

## <a name="management-api"></a>管理 API
使用管理 API 通过模板创建资源组和部署资源。

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a>示例 
在 Azure 美国东部区域中创建新的资源组。

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a>示例
[管理 Azure 资源和资源组](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

查看 Azure 资源管理器示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)。
