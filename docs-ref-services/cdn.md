---
title: "用于 Python 的 Azure CDN 库"
description: "用于 Python 的 Azure CDN 库参考"
keywords: Azure, python, SDK, API, CDN
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: c704b32ff5fd6db922ef9c296142832455088562
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cdn-libraries-for-python"></a><span data-ttu-id="305cd-104">用于 Python 的 Azure CDN 库</span><span class="sxs-lookup"><span data-stu-id="305cd-104">Azure CDN libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="305cd-105">概述</span><span class="sxs-lookup"><span data-stu-id="305cd-105">Overview</span></span>

<span data-ttu-id="305cd-106">使用 [Azure 内容交付网络 (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) 可以提供 Web 内容缓存，确保在全球范围内提供高带宽。</span><span class="sxs-lookup"><span data-stu-id="305cd-106">[Azure Content Delivery Network (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) allows you to provide web content caches to ensure high-bandwidth availability across the globe.</span></span>

<span data-ttu-id="305cd-107">若要开始使用 Azure CDN，请参阅 [Azure CDN 入门](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint)。</span><span class="sxs-lookup"><span data-stu-id="305cd-107">To get started with Azure CDN, see [Getting started with Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-apis"></a><span data-ttu-id="305cd-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="305cd-108">Management APIs</span></span>

<span data-ttu-id="305cd-109">使用管理 API 创建、查询和管理 Azure CDN。</span><span class="sxs-lookup"><span data-stu-id="305cd-109">Create, query, and manage Azure CDNs with the management API.</span></span>

<span data-ttu-id="305cd-110">通过 pip 安装管理包。</span><span class="sxs-lookup"><span data-stu-id="305cd-110">Install the management package via pip.</span></span>

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a><span data-ttu-id="305cd-111">示例</span><span class="sxs-lookup"><span data-stu-id="305cd-111">Example</span></span>

<span data-ttu-id="305cd-112">使用定义的单个终结点创建 CDN 配置文件：</span><span class="sxs-lookup"><span data-stu-id="305cd-112">Creating a CDN profile with a single defined endpoint:</span></span>

```python
from azure.mgmt.cdn import CdnManagementClient

cdn_client = CdnManagementClient(credentials, 'your-subscription-id')
profile_poller = cdn_client.profiles.create('my-resource-group',
                                            'cdn-name',
                                            {
                                                "location": "some_region", 
                                                "sku": {
                                                    "name": "sku_tier"
                                                } 
                                            })
profile = profile_poller.result()

endpoint_poller = client.endpoints.create('my-resource-group',
                                          'cdn-name',
                                          'unique-endpoint-name', 
                                          { 
                                              "location": "any_region", 
                                              "origins": [
                                                  {
                                                      "name": "origin_name", 
                                                      "host_name": "url"
                                                  }]
                                          })
endpoint = endpoint_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="305cd-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="305cd-113">Explore the Management APIs</span></span>](/python/api/overview/azure/cdn/managementlibrary)
