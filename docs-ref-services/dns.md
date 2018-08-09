---
title: 用于 Python 的 Azure DNS 库
description: 用于 Python 的 Azure DNS 库参考
keywords: Azure, python, SDK, API, DNS
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 0a92b191f245585fd27261e99bea6158ff127a80
ms.sourcegitcommit: 8a9e4295359a4f47b21908541e2460c333e94a0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2018
ms.locfileid: "39624953"
---
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="82aac-104">用于 Python 的 Azure DNS 库</span><span class="sxs-lookup"><span data-stu-id="82aac-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="82aac-105">概述</span><span class="sxs-lookup"><span data-stu-id="82aac-105">Overview</span></span>

<span data-ttu-id="82aac-106">[Azure DNS](/azure/dns/dns-overview) 是 DNS 域的托管服务，它通过 Azure 基础结构提供 DNS 解析。</span><span class="sxs-lookup"><span data-stu-id="82aac-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="82aac-107">若要开始使用 Azure DNS，请参阅[通过 Azure 门户开始使用 Azure DNS](/azure/dns/dns-getstarted-portal)。</span><span class="sxs-lookup"><span data-stu-id="82aac-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apipythonapioverviewazurednsmanagement"></a>[<span data-ttu-id="82aac-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="82aac-108">Management API</span></span>](/python/api/overview/azure/dns/management)

```bash
pip install azure-mgmt-dns
```

## <a name="create-the-management-client"></a><span data-ttu-id="82aac-109">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="82aac-109">Create the management client</span></span>

<span data-ttu-id="82aac-110">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="82aac-110">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="82aac-111">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="82aac-111">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="82aac-112">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="82aac-112">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python 
from azure.mgmt.dns import DnsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

dns_client = DnsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="create-dns-zone"></a><span data-ttu-id="82aac-113">创建 DNS 区域</span><span class="sxs-lookup"><span data-stu-id="82aac-113">Create DNS zone</span></span>
```python
# The only valid value is 'global', otherwise you will get a:
# The subscription is not registered for the resource type 'dnszones' in the location 'westus'.
zone = dns_client.zones.create_or_update(
    'MyResourceGroup',
    'pydns.com',
    {
            'zone_type': 'Public', # or Private
        'location': 'global'
    }
)
```
    
## <a name="create-a-record-set"></a><span data-ttu-id="82aac-114">创建记录集</span><span class="sxs-lookup"><span data-stu-id="82aac-114">Create a Record Set</span></span>
```python
record_set = dns_client.record_sets.create_or_update(
    'MyResourceGroup',
    'pydns.com',
    'MyRecordSet',
    'A',
    {
            "ttl": 300,
            "arecords": [
                {
                "ipv4_address": "1.2.3.4"
                },
                {
                "ipv4_address": "1.2.3.5"
                }
            ]
    }
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="82aac-115">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="82aac-115">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/management)
