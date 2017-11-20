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
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="b7e8c-104">用于 Python 的 Azure DNS 库</span><span class="sxs-lookup"><span data-stu-id="b7e8c-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="b7e8c-105">概述</span><span class="sxs-lookup"><span data-stu-id="b7e8c-105">Overview</span></span>

<span data-ttu-id="b7e8c-106">[Azure DNS](/azure/dns/dns-overview) 是 DNS 域的托管服务，它通过 Azure 基础结构提供 DNS 解析。</span><span class="sxs-lookup"><span data-stu-id="b7e8c-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="b7e8c-107">若要开始使用 Azure DNS，请参阅[通过 Azure 门户开始使用 Azure DNS](/azure/dns/dns-getstarted-portal)。</span><span class="sxs-lookup"><span data-stu-id="b7e8c-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apis"></a><span data-ttu-id="b7e8c-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="b7e8c-108">Management APIs</span></span>

<span data-ttu-id="b7e8c-109">使用管理 API 创建和管理 DNS 区域与记录。</span><span class="sxs-lookup"><span data-stu-id="b7e8c-109">Create and manage DNS zones and records with the management API.</span></span>

<span data-ttu-id="b7e8c-110">使用 pip 安装管理包。</span><span class="sxs-lookup"><span data-stu-id="b7e8c-110">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a><span data-ttu-id="b7e8c-111">示例</span><span class="sxs-lookup"><span data-stu-id="b7e8c-111">Example</span></span>

<span data-ttu-id="b7e8c-112">创建新的 DNS 区域。</span><span class="sxs-lookup"><span data-stu-id="b7e8c-112">Create a new DNS zone.</span></span>

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
> [<span data-ttu-id="b7e8c-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="b7e8c-113">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/managementlibrary)
