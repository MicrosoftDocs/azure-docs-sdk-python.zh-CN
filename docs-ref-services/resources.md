---
title: "用于 Python 的 Azure 资源库"
description: "用于 Python 的 Azure 资源库参考"
keywords: "Azure, Python, SDK, API, 资源"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f341e9066f48b35e182b4aba52b2f69e2b33e984
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
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

resource_client = ResourceManagmentClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/resources/managementlibrary)

## <a name="samples"></a>示例
[管理 Azure 资源和资源组](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

查看 Azure 资源管理器示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)。
