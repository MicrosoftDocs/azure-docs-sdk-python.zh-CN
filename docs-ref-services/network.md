---
title: 用于 Python 的 Azure 网络库
description: 用于 Python 的 Azure 网络库参考
keywords: Azure, python, SDK, API, 网络
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 47252ca3b2f5c6087277bac3735025f0dbabbdd8
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479070"
---
# <a name="azure-network-libraries-for-python"></a>用于 Python 的 Azure 网络库

## <a name="overview"></a>概述

使用 [Azure 虚拟网络](/azure/virtual-network/virtual-networks-overview)可以连接 Azure 资源，而且还能将它们连接到本地网络。

若要开始使用 Azure 虚拟网络，请参阅[创建第一个虚拟网络](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。

## <a name="management-apis"></a>管理 API

使用管理 API 检查、管理和配置 Azure 虚拟网络。

与其他 Azure Python API 不同，网络 API 在单独的包中受到显式版本控制。 不需要单独导入这些包，因为包信息已在客户端构造函数中指定。

使用 pip 安装管理包。

```bash
pip install azure-mgmt-network
```

### <a name="example"></a>示例

创建虚拟网络和关联的子网。

```python
from azure.mgmt.network import NetworkManagementClient

GROUP_NAME = 'resource-group'
VNET_NAME = 'your-vnet-identifier'
LOCATION = 'region'
SUBNET_NAME = 'your-subnet-identifier'

network_client = NetworkManagementClient(credentials, 'your-subscription-id')

async_vnet_creation = network_client.virtual_networks.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    {
        'location': LOCATION,
        'address_space': {
            'address_prefixes': ['10.0.0.0/16']
        }
    }
)
async_vnet_creation.wait()

# Create Subnet
async_subnet_creation = network_client.subnets.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    SUBNET_NAME,
    {'address_prefix': '10.0.0.0/24'}
)
subnet_info = async_subnet_creation.result()
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/network/management)

### <a name="samples"></a>示例

* [开始通过 Python 使用 Azure 资源管理器管理负载均衡器](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)

查看 Azure 虚拟网络示例的[完整列表](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network)。
