---
title: "用于 Python 的 Azure 虚拟机库"
description: 
keywords: "Azure, Python, SDK, API, 计算, 虚拟机"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: e2f2ad4e42bd847c9286333bacd583c3cd3f1b8c
ms.sourcegitcommit: 79afc8a1b427e26ecea7bdc0b7b3c898f143360f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="74054-103">Azure 虚拟机库</span><span class="sxs-lookup"><span data-stu-id="74054-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="74054-104">概述</span><span class="sxs-lookup"><span data-stu-id="74054-104">Overview</span></span>

<span data-ttu-id="74054-105">运行 Linux 或 Windows 的按需可缩放计算资源。</span><span class="sxs-lookup"><span data-stu-id="74054-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="74054-106">若要开始使用 Azure 虚拟机，请参阅[使用 Azure 门户创建 Linux 虚拟机](/azure/virtual-machines/linux/quick-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="74054-106">To get started with Azure Virtual Machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="74054-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="74054-107">Management API</span></span>

<span data-ttu-id="74054-108">使用管理 API 通过代码在 Azure 中创建、配置、管理和缩放 Windows 与 Linux 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="74054-108">Create, configure, manage and scale Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="74054-109">通过 pip 安装库。</span><span class="sxs-lookup"><span data-stu-id="74054-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a><span data-ttu-id="74054-110">示例</span><span class="sxs-lookup"><span data-stu-id="74054-110">Example</span></span>

<span data-ttu-id="74054-111">使用托管服务标识 (MSI) 身份验证在现有 Azure 资源组中创建新的 Linux 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="74054-111">Create a new Linux virtual machine in an existing Azure resource group with Managed Service Identity(MSI) authentication.</span></span>

```python
VM_PARAMETERS={
        'location': 'LOCATION',
        'os_profile': {
            'computer_name': 'VM_NAME',
            'admin_username': 'USERNAME',
            'admin_password': 'PASSWORD'
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': 'Canonical',
                'offer': 'UbuntuServer',
                'sku': '16.04.0-LTS',
                'version': 'latest'
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': 'NIC_ID',
            }]
        },
    }

def create_vm()
    compute_client.virtual_machines.create_or_update(
        'RESOURCE_GROUP_NAME', 'VM_NAME', VM_PARAMETERS)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="74054-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="74054-112">Explore the Management APIs</span></span>](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a><span data-ttu-id="74054-113">示例</span><span class="sxs-lookup"><span data-stu-id="74054-113">Samples</span></span>

* <span data-ttu-id="74054-114">[管理虚拟机][1]</span><span class="sxs-lookup"><span data-stu-id="74054-114">[Manage virtual machines][1]</span></span>
* <span data-ttu-id="74054-115">[使用托管服务标识 (MSI) 进行身份验证][2]</span><span class="sxs-lookup"><span data-stu-id="74054-115">[Authenticate with Managed Service Identity][2]</span></span>
* <span data-ttu-id="74054-116">[管理负载均衡器][3]</span><span class="sxs-lookup"><span data-stu-id="74054-116">[Manage a load balancer][3]</span></span>
* <span data-ttu-id="74054-117">[创建和配置托管磁盘][4]</span><span class="sxs-lookup"><span data-stu-id="74054-117">[Create and configure managed disks][4]</span></span>
* <span data-ttu-id="74054-118">[列出映像][5]</span><span class="sxs-lookup"><span data-stu-id="74054-118">[List images][5]</span></span> 
* <span data-ttu-id="74054-119">[监视虚拟机][6]</span><span class="sxs-lookup"><span data-stu-id="74054-119">[Monitor virtual machines][6]</span></span>

<span data-ttu-id="74054-120">查看虚拟机示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)。</span><span class="sxs-lookup"><span data-stu-id="74054-120">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) of virtual machine samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://github.com/Azure-Samples/resource-manager-python-manage-resources-with-msi
[3]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[4]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[6]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md