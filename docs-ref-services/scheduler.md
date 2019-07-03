---
title: 用于 Python 的 Azure 计划程序库
description: 用于 Python 的 Azure 计划程序库参考
keywords: Azure, python, SDK, API, 计划程序
author: lisawong19
ms.author: routlaw
manager: mbaldwin
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 73c33ff808212fade192ca7c7c05a8e64d3fafda
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534214"
---
# <a name="azure-scheduler-libraries-for-python"></a><span data-ttu-id="a977c-104">用于 Python 的 Azure 计划程序库</span><span class="sxs-lookup"><span data-stu-id="a977c-104">Azure Scheduler libraries for python</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="a977c-105">安装库</span><span class="sxs-lookup"><span data-stu-id="a977c-105">Install the libraries</span></span>

## <a name="management"></a><span data-ttu-id="a977c-106">管理</span><span class="sxs-lookup"><span data-stu-id="a977c-106">Management</span></span>

```bash
pip install azure-mgmt-scheduler
```
## <a name="example"></a><span data-ttu-id="a977c-107">示例</span><span class="sxs-lookup"><span data-stu-id="a977c-107">Example</span></span>

### <a name="create-the-management-client"></a><span data-ttu-id="a977c-108">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="a977c-108">Create the management client</span></span>

<span data-ttu-id="a977c-109">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="a977c-109">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="a977c-110">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="a977c-110">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="a977c-111">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="a977c-111">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.scheduler import SchedulerManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

scheduler_client = SchedulerManagementClient(
    credentials,
    subscription_id
)
```

### <a name="create-a-job-collection"></a><span data-ttu-id="a977c-112">创建作业集合</span><span class="sxs-lookup"><span data-stu-id="a977c-112">Create a job collection</span></span>

<span data-ttu-id="a977c-113">下面的代码在现有的资源组下创建作业集合。</span><span class="sxs-lookup"><span data-stu-id="a977c-113">The following code creates a job collection under an existing resource group.</span></span>
<span data-ttu-id="a977c-114">若要创建或管理资源组，请参阅[资源管理](/python/api/overview/azure/azure.mgmt.resource)。</span><span class="sxs-lookup"><span data-stu-id="a977c-114">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.scheduler.models import JobCollectionDefinition, JobCollectionProperties, Sku

group_name = 'myresourcegroup'
job_collection_name = "myjobcollection"
scheduler_client.job_collections.create_or_update(
    group_name,
    job_collection_name,
    JobCollectionDefinition(
        location = "West US",
        properties = JobCollectionProperties(
            sku = Sku(
                name="Free"
            )
        )
    )
)
# scheduler_client is a JobCollectionDefinition instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="a977c-115">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="a977c-115">Explore the Management APIs</span></span>](/python/api/overview/azure/scheduler/management)