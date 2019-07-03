---
title: 用于 Python 的 Azure 资源库
description: ''
keywords: Azure, Python, SDK, API, 资源
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: d708a5e7296b166b6e55b9b7b0d995e72e264267
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534372"
---
# <a name="azure-resources-libraries-for-python"></a>用于 Python 的 Azure 资源库 

## <a name="overview"></a>概述 
在资源组中管理 Azure 资源。

| 程序包  |  说明 |
|---|---|
|[azure.mgmt.resource.features][1]|Azure 功能公开控制 (AFEC) 提供一个机制让资源提供程序控制公开给用户的功能。|
|[azure.mgmt.resource.links][2]|Azure 资源可以链接在一起，以构成逻辑关系。 可以在属于不同资源组的资源之间建立链接。|
|[azure.mgmt.resource.locks][3]|可以锁定 Azure 资源，防止组织中的其他用户删除或修改资源。|
|[azure.mgmt.resource.managedapplications][4]|ARM 托管应用程序（设备）。|
|[azure.mgmt.resource.policy][5]|若要管理和控制对资源的访问，可以定义并在某个范围内分配自定义的策略。|
|[azure.mgmt.resource.resources][6]| 提供用于处理资源和资源组的操作。|
|[azure.mgmt.resource.subscriptions][7]|所有资源组和资源都在订阅中。 使用这些操作可以获取有关订阅和租户的信息。|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a>安装库 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a>示例
以下示例演示如何创建资源组。 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

详细了解可在应用中使用的[示例 Python 代码](https://azure.microsoft.com/resources/samples/?platform=python)。 