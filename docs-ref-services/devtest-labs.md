---
title: 用于 Python 的 Azure 开发测试实验室库
description: 用于 Python 的 Azure 开发测试实验室库参考
keywords: Azure, python, SDK, API, 开发测试实验室
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 80c9b90c9527503f9cc3782b429b4da1d4ff514f
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277001"
---
# <a name="azure-devtest-labs-libraries-for-python"></a><span data-ttu-id="aa687-104">用于 Python 的 Azure 开发测试实验室库</span><span class="sxs-lookup"><span data-stu-id="aa687-104">Azure DevTest Labs libraries for python</span></span>

## <a name="management-apipythonapioverviewazuredevtestlabsmanagement"></a>[<span data-ttu-id="aa687-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="aa687-105">Management API</span></span>](/python/api/overview/azure/devtestlabs/management)

```bash
pip install azure-mgmt-devtestlabs
```

## <a name="create-the-management-client"></a><span data-ttu-id="aa687-106">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="aa687-106">Create the management client</span></span>

<span data-ttu-id="aa687-107">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="aa687-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="aa687-108">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="aa687-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="aa687-109">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="aa687-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.devtestlabs import DevTestLabsClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

devtestlabs_client = DevTestLabsClient(
    credentials,
    subscription_id
)
```

## <a name="create-lab"></a><span data-ttu-id="aa687-110">创建实验室</span><span class="sxs-lookup"><span data-stu-id="aa687-110">Create lab</span></span>

```python
async_lab = self.client.lab.create_or_update_resource(
    'MyResourceGroup',
    'MyLab',
    {'location': 'westus'}
)
lab = async_lab.result() # Blocking wait
``` 

> [!div class="nextstepaction"]
> [<span data-ttu-id="aa687-111">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="aa687-111">Explore the Management APIs</span></span>](/python/api/overview/azure/devtestlabs/management)