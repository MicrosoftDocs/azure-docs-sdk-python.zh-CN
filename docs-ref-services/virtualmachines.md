---
title: 用于 Python 的 Azure 虚拟机库
description: ''
keywords: Azure, Python, SDK, API, 计算, 虚拟机
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: 78750d5f98ab81401c48493aff98d4268c01850d
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376702"
---
# <a name="azure-virtual-machine-libraries"></a>Azure 虚拟机库

## <a name="overview"></a>概述

运行 Linux 或 Windows 的按需可缩放计算资源。

若要开始使用 Azure 虚拟机，请参阅[使用 Azure 门户创建 Linux 虚拟机](/azure/virtual-machines/linux/quick-create-portal)。

## <a name="management-api"></a>管理 API

使用管理 API 通过代码在 Azure 中创建、配置、管理和缩放 Windows 与 Linux 虚拟机。

通过 pip 安装库。

```bash
pip install azure-mgmt-compute
```

### <a name="example"></a>示例

使用托管服务标识 (MSI) 身份验证在现有 Azure 资源组中创建新的 Linux 虚拟机。

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
> [了解管理 API](/python/api/overview/azure/virtualmachines/management)

## <a name="samples"></a>示例

* [管理虚拟机][1]
* [使用托管服务标识 (MSI) 进行身份验证][2]
* [使用托管服务标识扩展创建虚拟机][3]
* [管理负载均衡器][4]
* [创建和配置托管磁盘][5]
* [列出映像][6] 
* [监视虚拟机][7]

查看虚拟机示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)。

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://github.com/Azure-Samples/resource-manager-python-manage-resources-with-msi
[3]: https://github.com/Azure-Samples/compute-python-msi-vm
[4]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[6]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[7]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md
