---
title: "托管磁盘"
description: "创建、调整和更新托管磁盘。"
author: lisawong19
manager: douge
ms.assetid: 
ms.devlang: python
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 6/15/2017
ms.author: liwong
ms.openlocfilehash: 776e13ed91482c34e5d637d5eedf2640cd4ca9f4
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="managed-disks"></a>托管磁盘

Azure 托管磁盘和规模集中的 1000 个 VM 现已推出[正式版](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-managed-disks-and-larger-scale-sets/)。Azure 托管磁盘提供简化的磁盘管理、增强的可伸缩性以及更高的安全性和缩放能力。 它消除了磁盘的存储帐户概念，使客户能够进行缩放，而无需担心存储帐户相关的限制。 本文提供有关从 Python 使用该服务的快速简介和参考信息。



从开发人员的角度来看，Azure CLI 中的托管磁盘体验与其他跨平台工具中的 CLI 体验有异曲同工之处。 可以使用 [Azure Python](https://azure.microsoft.com/develop/python/) SDK 和 [azure-mgmt-compute 包 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) 来管理托管磁盘。 可以参考[此教程](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementcomputenetwork.html)创建计算客户端。


## <a name="standalone-managed-disks"></a>独立托管磁盘

可通过多种方式轻松创建独立托管磁盘。

### <a name="create-an-empty-managed-disk"></a>创建空托管磁盘。
```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>从 Blob 存储创建托管磁盘。
```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>从自己的映像创建托管磁盘
```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')
async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = async_creation.result()
```

## <a name="virtual-machine-with-managed-disks"></a>包含托管磁盘的虚拟机

可以根据特定的磁盘映像创建包含隐式托管磁盘的虚拟机。 在不指定所有磁盘详细信息的情况下隐式创建托管磁盘简化了创建过程。 无需担心如何创建和管理存储帐户。

从 Azure 中的 OS 映像创建 VM 时，会隐式创建托管磁盘。 在 ``storage_profile`` 参数中，``os_disk`` 现在是可选的；而在过去，创建虚拟机之前必须事先创建存储帐户。

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
``` 
此 ``storage_profile`` 参数现在有效。 若要获取有关如何在 Python 中创建 VM（包括网络等）的完整示例，请查看完整的 [Python 中的 VM 教程](https://github.com/Azure-Samples/virtual-machines-python-manage)。

可以轻松附加以前预配的托管磁盘。
```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOption.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})
async_update = compute_client.virtual_machines.create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>带托管磁盘的虚拟机规模集

在托管磁盘推出之前，需针对要放入规模集的所有 VM 手动创建存储帐户，然后使用列表参数 ``vhd_containers`` 将所有存储帐户名称提供给规模集 RestAPI。 此文 (`article <https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`) 中提供了官方转换指南。

现在，有了托管磁盘，不需要管理任何存储帐户。 如果熟悉 VMSS Python SDK 的话，``storage_profile`` 与创建 VM 时所用的配置文件完全相同：

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

完整示例：

```python
naming_infix = "PyTestInfix"
vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    } 
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
``` 

## <a name="other-operations-with-managed-disks"></a>可对托管磁盘执行的其他操作

### <a name="resizing-a-managed-disk"></a>调整托管磁盘的大小。

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>更新托管磁盘的存储帐户类型。
```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-blob-storage"></a>从 Blob 存储创建映像。
```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>创建当前已附加到虚拟机的托管磁盘的快照。
```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
async_snapshot_creation = self.compute_client.snapshots.create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```
