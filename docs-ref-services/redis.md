---
title: "用于 Python 的 Azure Redis 库"
description: "用于 Redis 的 Python 客户端库的参考文档"
keywords: "Azure, Python, Redis, API, SDK, 数据库, NoSQL"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: redis
ms.openlocfilehash: c2f8ebcbcbd7b71c1fa96e46a8148a3c0b88877f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="azure-redis-cache-libraries-for-python"></a>用于 Python 的 Azure Redis 缓存库

## <a name="overview"></a>概述

Azure Redis 缓存基于流行的开源 Redis 项目。 使用它可以访问 Microsoft 管理的、可从 Azure 应用访问的安全专用 Redis 实例。

Redis 是一种高级键值存储，其中键可包含数据结构，例如字符串、哈希、列表、集和排序集。 Redis 支持针对这些数据类型的一组原子操作。

详细了解 [Azure Redis 缓存](https://docs.microsoft.com/azure/redis-cache/)。

## <a name="management-api"></a>管理 API

使用 Redis 管理 API 在订阅中创建和管理 Redis 资源。

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a>示例

以下示例创建新的 Redis 缓存：

```python
from azure.mgmt.redis import RedisManagementClient
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

redis_client = RedisManagementClient(
    credentials,
    subscription_id
)
group_name = 'myresourcegroup'
cache_name = 'mycachename'
redis_cache = redis_client.redis.create_or_update(
    group_name,
    cache_name,
    RedisCreateOrUpdateParameters(
        sku = Sku(name = 'Basic', family = 'C', capacity = '1'),
        location = "East US"
    )
)
# redis_cache is a RedisResourceWithAccessKey instance
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/redis/management)

