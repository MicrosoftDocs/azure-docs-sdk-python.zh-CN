---
title: 用于 Python 的 Azure 通知中心库
description: 用于 Python 的 Azure 通知中心库参考
keywords: Azure, Python, SDK, API, 通知中心
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: ea5ef3722635484d23459d1d39ec6216d4574b85
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376907"
---
# <a name="azure-notification-hubs-libraries-for-python"></a><span data-ttu-id="73124-104">用于 Python 的 Azure 通知中心库</span><span class="sxs-lookup"><span data-stu-id="73124-104">Azure Notification Hubs libraries for python</span></span>

## <a name="management-apipythonapioverviewazurenotificationhubsmanagement"></a>[<span data-ttu-id="73124-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="73124-105">Management API</span></span>](/python/api/overview/azure/notificationhubs/management)

```bash
pip install azure-mgmt-notificationhubs
```

## <a name="create-the-management-client"></a><span data-ttu-id="73124-106">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="73124-106">Create the management client</span></span>

<span data-ttu-id="73124-107">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="73124-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="73124-108">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="73124-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="73124-109">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="73124-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.notificationhubs import NotificationHubsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

redis_client = NotificationHubsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-namespace-availability"></a><span data-ttu-id="73124-110">检查命名空间可用性</span><span class="sxs-lookup"><span data-stu-id="73124-110">Check namespace availability</span></span>

<span data-ttu-id="73124-111">以下代码检查通知中心的命名空间可用性。</span><span class="sxs-lookup"><span data-stu-id="73124-111">The following code check namespace availability of a notification hub.</span></span>

```python
from azure.mgmt.notificationhubs.models import CheckAvailabilityParameters

account_name = 'mynotificationhub'
output = notificationhubs_client.namespaces.check_availability(
    azure.mgmt.notificationhubs.models.CheckAvailabilityParameters(
        name = account_name
    )
)
# output is a CheckAvailibilityResource instance
print(output.is_availiable) # Yes, it's 'availiable', it's a typo in the REST API
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="73124-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="73124-112">Explore the Management APIs</span></span>](/python/api/overview/azure/notificationhubs/management)
