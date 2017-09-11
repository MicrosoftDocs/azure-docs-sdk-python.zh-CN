---
title: "用于 Python 的 Azure DNS 库"
description: "用于 Python 的 Azure DNS 库参考"
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
ms.openlocfilehash: 59ae3628b4a810db8fee21aacf46c13054dc8cd3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-dns-libraries-for-python"></a>用于 Python 的 Azure DNS 库

## <a name="overview"></a>概述

[Azure DNS](/azure/dns/dns-overview) 是 DNS 域的托管服务，它通过 Azure 基础结构提供 DNS 解析。

若要开始使用 Azure DNS，请参阅[通过 Azure 门户开始使用 Azure DNS](/azure/dns/dns-getstarted-portal)。

## <a name="management-apis"></a>管理 API

使用管理 API 创建和管理 DNS 区域与记录。

使用 pip 安装管理包。

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a>示例

创建新的 DNS 区域。

```python
from azure.mgmt.dns import DnsManagementClient

dns_client = DnsManagementClient(credentials, 'your-subscription-id')
zone = dns_client.zones.create_or_update('resource-group',
                                         'zone_name_no_dot',
                                         {
                                            "location": "global"
                                         })

```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/dns/managementlibrary)
