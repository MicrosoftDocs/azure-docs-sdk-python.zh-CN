---
title: "用于 Python 的 Azure 网络库"
description: "用于 Python 的 Azure 网络库参考"
keywords: "Azure, python, SDK, API, 网络"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: ed6ddfe4d1f4d860952369a75e10166a1f22c483
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-network-libraries-for-python"></a><span data-ttu-id="9d1cd-104">用于 Python 的 Azure 网络库</span><span class="sxs-lookup"><span data-stu-id="9d1cd-104">Azure Network libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="9d1cd-105">概述</span><span class="sxs-lookup"><span data-stu-id="9d1cd-105">Overview</span></span>

<span data-ttu-id="9d1cd-106">使用 [Azure 虚拟网络](/azure/virtual-network/virtual-networks-overview)可以连接 Azure 资源，而且还能将它们连接到本地网络。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-106">[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) allows you to connect Azure resources, and also connect them to your on-premises network.</span></span>

<span data-ttu-id="9d1cd-107">若要开始使用 Azure 虚拟网络，请参阅[创建第一个虚拟网络](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-107">To get started with Azure Virtual Network, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-apis"></a><span data-ttu-id="9d1cd-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="9d1cd-108">Management APIs</span></span>

<span data-ttu-id="9d1cd-109">使用管理 API 检查、管理和配置 Azure 虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-109">Inspect, manage, and configure Azure virtual networks with the management APIs.</span></span>

<span data-ttu-id="9d1cd-110">与其他 Azure Python API 不同，网络 API 在单独的包中受到显式版本控制。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-110">Unlike other Azure python APIs, the networking APIs are explicitly versioned into separage packages.</span></span> <span data-ttu-id="9d1cd-111">不需要单独导入这些包，因为包信息已在客户端构造函数中指定。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-111">You do not need to import them individually since the package information is specified in the client constructor.</span></span>

<span data-ttu-id="9d1cd-112">使用 pip 安装管理包。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-112">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-network
```

### <a name="example"></a><span data-ttu-id="9d1cd-113">示例</span><span class="sxs-lookup"><span data-stu-id="9d1cd-113">Example</span></span>

<span data-ttu-id="9d1cd-114">创建虚拟网络和关联的子网。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-114">Create a virtual network and an associated subnet.</span></span>

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
> [<span data-ttu-id="9d1cd-115">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="9d1cd-115">Explore the Management APIs</span></span>](/python/api/overview/azure/network/managementlibrary)

### <a name="samples"></a><span data-ttu-id="9d1cd-116">示例</span><span class="sxs-lookup"><span data-stu-id="9d1cd-116">Samples</span></span>

* <span data-ttu-id="9d1cd-117">[在 Python 中开始使用负载均衡器的 Azure 资源管理器][1]</span><span class="sxs-lookup"><span data-stu-id="9d1cd-117">[Getting started with Azure Resource Manager for load balancers in Python][1]</span></span>

<span data-ttu-id="9d1cd-118">查看 Azure 虚拟网络示例的[完整列表](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network)。</span><span class="sxs-lookup"><span data-stu-id="9d1cd-118">View the [complete list](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) of Azure Virtual Network samples.</span></span>

[1]: [https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/]
