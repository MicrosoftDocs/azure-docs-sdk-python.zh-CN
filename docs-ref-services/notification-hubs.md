---
title: "用于 Python 的 Azure 通知中心库"
description: "用于 Python 的 Azure 通知中心库参考"
keywords: "Azure, Python, SDK, API, 通知中心"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 66b452a40fd524672f4dad92a9d1bd0ffb77a99d
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
---
# <a name="azure-notification-hubs-libraries-for-python"></a>用于 Python 的 Azure 通知中心库

## <a name="management-apipythonapioverviewazurenotificationhubsmanagement"></a>[管理 API](/python/api/overview/azure/notificationhubs/management)

```bash
pip install azure-mgmt-notificationhubs
```

## <a name="create-the-management-client"></a>创建管理客户端

以下代码创建管理客户端的实例。

你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。

有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。

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

## <a name="check-namespace-availability"></a>检查命名空间可用性

以下代码检查通知中心的命名空间可用性。
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
> [了解管理 API](/python/api/overview/azure/notificationhubs/management)
