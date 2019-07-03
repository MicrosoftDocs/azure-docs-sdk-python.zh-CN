---
title: 用于 Python 的 Azure 授权库
description: 用于 Python 的 Azure 授权库参考
keywords: Azure, python, SDK, API, 授权
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: c7c4af34e82f2baad39635ecd20f1b9c48900b41
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534360"
---
# <a name="azure-authorization-libraries-for-python"></a><span data-ttu-id="054b9-104">用于 Python 的 Azure 授权库</span><span class="sxs-lookup"><span data-stu-id="054b9-104">Azure Authorization libraries for python</span></span>

## <a name="management-apipythonapioverviewazureauthorizationmanagement"></a>[<span data-ttu-id="054b9-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="054b9-105">Management API</span></span>](/python/api/overview/azure/authorization/management)

```bash
pip install azure-mgmt-authorization
```

## <a name="create-the-management-client"></a><span data-ttu-id="054b9-106">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="054b9-106">Create the management client</span></span>

<span data-ttu-id="054b9-107">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="054b9-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="054b9-108">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="054b9-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="054b9-109">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="054b9-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.authorization import AuthorizationManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password'       # Your password
)

authorization_client = AuthorizationManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-permissions-for-a-resource-group"></a><span data-ttu-id="054b9-110">检查资源组的权限</span><span class="sxs-lookup"><span data-stu-id="054b9-110">Check permissions for a resource group</span></span>

<span data-ttu-id="054b9-111">下面的代码检查给定的资源组中的权限。</span><span class="sxs-lookup"><span data-stu-id="054b9-111">The following code checks permissions in a given resource group.</span></span> <span data-ttu-id="054b9-112">若要创建或管理资源组，请参阅[资源管理](/python/api/overview/azure/azure.mgmt.resource)。</span><span class="sxs-lookup"><span data-stu-id="054b9-112">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

group_name = 'myresourcegroup'
permissions = self.authorization_client.permissions.list_for_resource_group(
    group_name
)
# permissions is a iterable of Permissions instances
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="054b9-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="054b9-113">Explore the Management APIs</span></span>](/python/api/overview/azure/authorization/management)
