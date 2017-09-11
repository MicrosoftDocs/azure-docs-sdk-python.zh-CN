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
# <a name="azure-cdn-libraries-for-python"></a>用于 Python 的 Azure CDN 库

## <a name="overview"></a>概述

使用 [Azure 内容交付网络 (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) 可以提供 Web 内容缓存，确保在全球范围内提供高带宽。

若要开始使用 Azure CDN，请参阅 [Azure CDN 入门](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint)。

## <a name="management-apis"></a>管理 API

使用管理 API 创建、查询和管理 Azure CDN。

通过 pip 安装管理包。

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a>示例

使用定义的单个终结点创建 CDN 配置文件：

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
> [了解管理 API](/python/api/overview/azure/cdn/managementlibrary)
