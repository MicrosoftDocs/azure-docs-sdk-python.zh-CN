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
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="53a1e-104">用于 Python 的 Azure 资源库</span><span class="sxs-lookup"><span data-stu-id="53a1e-104">Azure Resources libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="53a1e-105">概述</span><span class="sxs-lookup"><span data-stu-id="53a1e-105">Overview</span></span> 
<span data-ttu-id="53a1e-106">使用 [Azure 资源管理器](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)部署、监视和管理组中的资源。</span><span class="sxs-lookup"><span data-stu-id="53a1e-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="53a1e-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="53a1e-107">Management API</span></span>
<span data-ttu-id="53a1e-108">使用管理 API 通过模板创建资源组和部署资源。</span><span class="sxs-lookup"><span data-stu-id="53a1e-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a><span data-ttu-id="53a1e-109">示例</span><span class="sxs-lookup"><span data-stu-id="53a1e-109">Example</span></span> 
<span data-ttu-id="53a1e-110">在 Azure 美国东部区域中创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="53a1e-110">Create a new resource group in the Azure Eastern US region.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagmentClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="53a1e-111">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="53a1e-111">Explore the Management APIs</span></span>](/python/api/overview/azure/resources/managementlibrary)

## <a name="samples"></a><span data-ttu-id="53a1e-112">示例</span><span class="sxs-lookup"><span data-stu-id="53a1e-112">Samples</span></span>
[<span data-ttu-id="53a1e-113">管理 Azure 资源和资源组</span><span class="sxs-lookup"><span data-stu-id="53a1e-113">Manage Azure resources and resource groups</span></span>](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

<span data-ttu-id="53a1e-114">查看 Azure 资源管理器示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)。</span><span class="sxs-lookup"><span data-stu-id="53a1e-114">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) of Azure Resource Manager samples.</span></span>
