---
title: 用于 Python 的 Azure 服务器管理器库
description: 用于 Python 的 Azure 服务器管理器库参考
keywords: Azure, python, SDK, API, 服务器管理器
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 480513a76683c97fdac8d2a65b38fde0fffb6dcd
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479230"
---
# <a name="azure-server-manager-libraries-for-python"></a><span data-ttu-id="18f10-104">用于 Python 的 Azure 服务器管理器库</span><span class="sxs-lookup"><span data-stu-id="18f10-104">Azure Server Manager libraries for python</span></span>

## <a name="management-apipythonapioverviewazureservermanagermanagement"></a>[<span data-ttu-id="18f10-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="18f10-105">Management API</span></span>](/python/api/overview/azure/servermanager/management)

```bash
pip install azure-mgmt-servermanager
```

## <a name="create-the-management-client"></a><span data-ttu-id="18f10-106">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="18f10-106">Create the management client</span></span>

<span data-ttu-id="18f10-107">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="18f10-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="18f10-108">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="18f10-108">You will need to provide your ``subscription_id`` which can be retrieved from your [subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="18f10-109">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="18f10-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.servermanager import ServerManagement
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

servermanager_client = ServerManagement(
    credentials,
    subscription_id
)
``` 

## <a name="create-gateway"></a><span data-ttu-id="18f10-110">创建网关</span><span class="sxs-lookup"><span data-stu-id="18f10-110">Create gateway</span></span>
```python
gateway_async = servermanager_client.gateway.create(
    'MyResourceGroup',
    'MyGateway',
    'centralus'
)
gateway = gateway_async.result() # Blocking wait
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="18f10-111">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="18f10-111">Explore the Management APIs</span></span>](/python/api/overview/azure/servermanager/management)