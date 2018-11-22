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
ms.openlocfilehash: c70c3d44911b28b0a8c92b5f7efeca3bf1611f43
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277249"
---
# <a name="azure-dns-libraries-for-python"></a>用于 Python 的 Azure DNS 库

## <a name="overview"></a>概述

[Azure DNS](/azure/dns/dns-overview) 是 DNS 域的托管服务，它通过 Azure 基础结构提供 DNS 解析。

若要开始使用 Azure DNS，请参阅[通过 Azure 门户开始使用 Azure DNS](/azure/dns/dns-getstarted-portal)。

## <a name="management-apipythonapioverviewazurednsmanagement"></a>[管理 API](/python/api/overview/azure/dns/management)

```bash
pip install azure-mgmt-dns
```

## <a name="create-the-management-client"></a>创建管理客户端

以下代码创建管理客户端的实例。

你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。

有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。

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

## <a name="create-dns-zone"></a>创建 DNS 区域
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
    
## <a name="create-a-record-set"></a>创建记录集
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
> [了解管理 API](/python/api/overview/azure/dns/management)
