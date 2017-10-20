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
ms.openlocfilehash: 1dceb1b2fe700904b530f1834f0338f7d5e61999
ms.sourcegitcommit: 3e477d608bbb41f0c561c88e4c665013e3008c26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="managed-disks"></a><span data-ttu-id="3248c-103">托管磁盘</span><span class="sxs-lookup"><span data-stu-id="3248c-103">Managed Disks</span></span>

<span data-ttu-id="3248c-104">Azure 托管磁盘和规模集中的 1000 个 VM 现已推出[正式版](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-managed-disks-and-larger-scale-sets/)。Azure 托管磁盘提供简化的磁盘管理、增强的可伸缩性以及更高的安全性和缩放能力。</span><span class="sxs-lookup"><span data-stu-id="3248c-104">Azure Managed Disks and 1000 VMs in a Scale Set are now [generally available](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-managed-disks-and-larger-scale-sets/) Azure Managed Disks provide a simplified disk Management, enhanced Scalability, better Security and Scale.</span></span> <span data-ttu-id="3248c-105">它消除了磁盘的存储帐户概念，使客户能够进行缩放，而无需担心存储帐户相关的限制。</span><span class="sxs-lookup"><span data-stu-id="3248c-105">It takes away the notion of storage account for disks, enabling customers to scale without worrying about the limitations associated with storage accounts.</span></span> <span data-ttu-id="3248c-106">本文提供有关从 Python 使用该服务的快速简介和参考信息。</span><span class="sxs-lookup"><span data-stu-id="3248c-106">This post provides a quick introduction and reference on consuming the service from Python.</span></span>



<span data-ttu-id="3248c-107">从开发人员的角度来看，Azure CLI 中的托管磁盘体验与其他跨平台工具中的 CLI 体验有异曲同工之处。</span><span class="sxs-lookup"><span data-stu-id="3248c-107">From a developer perspective, the Managed Disks experience in Azure CLI is idomatic to the CLI experience in other cross-platform tools.</span></span> <span data-ttu-id="3248c-108">可以使用 [Azure Python](https://azure.microsoft.com/develop/python/) SDK 和 [azure-mgmt-compute 包 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) 来管理托管磁盘。</span><span class="sxs-lookup"><span data-stu-id="3248c-108">You can use the [Azure Python](https://azure.microsoft.com/develop/python/) SDK and the [azure-mgmt-compute package 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) to administer Managed Disks.</span></span> <span data-ttu-id="3248c-109">可以参考[此教程](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementcomputenetwork.html)创建计算客户端。</span><span class="sxs-lookup"><span data-stu-id="3248c-109">You can create a compute client using this [tutorial](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementcomputenetwork.html).</span></span>


## <a name="standalone-managed-disks"></a><span data-ttu-id="3248c-110">独立托管磁盘</span><span class="sxs-lookup"><span data-stu-id="3248c-110">Standalone Managed Disks</span></span>

<span data-ttu-id="3248c-111">可通过多种方式轻松创建独立托管磁盘。</span><span class="sxs-lookup"><span data-stu-id="3248c-111">You can easily create standalone Managed Disks in a variety of ways.</span></span>

### <a name="create-an-empty-managed-disk"></a><span data-ttu-id="3248c-112">创建空托管磁盘。</span><span class="sxs-lookup"><span data-stu-id="3248c-112">Create an empty Managed Disk.</span></span>
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

### <a name="create-a-managed-disk-from-blob-storage"></a><span data-ttu-id="3248c-113">从 Blob 存储创建托管磁盘。</span><span class="sxs-lookup"><span data-stu-id="3248c-113">Create a Managed Disk from Blob Storage.</span></span>
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

### <a name="create-a-managed-disk-from-your-own-image"></a><span data-ttu-id="3248c-114">从自己的映像创建托管磁盘</span><span class="sxs-lookup"><span data-stu-id="3248c-114">Create a Managed Disk from your own Image</span></span>
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

## <a name="virtual-machine-with-managed-disks"></a><span data-ttu-id="3248c-115">包含托管磁盘的虚拟机</span><span class="sxs-lookup"><span data-stu-id="3248c-115">Virtual Machine with Managed Disks</span></span>

<span data-ttu-id="3248c-116">可以根据特定的磁盘映像创建包含隐式托管磁盘的虚拟机。</span><span class="sxs-lookup"><span data-stu-id="3248c-116">You can create a Virtual Machine with an implicit Managed Disk for a specific disk image.</span></span> <span data-ttu-id="3248c-117">在不指定所有磁盘详细信息的情况下隐式创建托管磁盘简化了创建过程。</span><span class="sxs-lookup"><span data-stu-id="3248c-117">Creation is simplified with implicit creation of managed disks without specifying all the disk details.</span></span> <span data-ttu-id="3248c-118">无需担心如何创建和管理存储帐户。</span><span class="sxs-lookup"><span data-stu-id="3248c-118">You do not have to worry about creating and managing Storage Accounts.</span></span>

<span data-ttu-id="3248c-119">从 Azure 中的 OS 映像创建 VM 时，会隐式创建托管磁盘。</span><span class="sxs-lookup"><span data-stu-id="3248c-119">A Managed Disk is created implicitly when creating VM from an OS image in Azure.</span></span> <span data-ttu-id="3248c-120">在 ``storage_profile`` 参数中，``os_disk`` 现在是可选的；而在过去，创建虚拟机之前必须事先创建存储帐户。</span><span class="sxs-lookup"><span data-stu-id="3248c-120">In the ``storage_profile`` parameter, ``os_disk`` is now optional and you don't have to create a storage account as required precondition to create a Virtual Machine.</span></span>

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
<span data-ttu-id="3248c-121">此 ``storage_profile`` 参数现在有效。</span><span class="sxs-lookup"><span data-stu-id="3248c-121">This ``storage_profile`` parameter is now valid.</span></span> <span data-ttu-id="3248c-122">若要获取有关如何在 Python 中创建 VM（包括网络等）的完整示例，请查看完整的 [Python 中的 VM 教程](https://github.com/Azure-Samples/virtual-machines-python-manage)。</span><span class="sxs-lookup"><span data-stu-id="3248c-122">To get a complete example on how to create a VM in Python (including network, etc), check the full [VM tutorial in Python](https://github.com/Azure-Samples/virtual-machines-python-manage).</span></span>

<span data-ttu-id="3248c-123">可以轻松附加以前预配的托管磁盘。</span><span class="sxs-lookup"><span data-stu-id="3248c-123">You can easily attach a previously provisioned Managed Disk.</span></span>
```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
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

## <a name="virtual-machine-scale-sets-with-managed-disks"></a><span data-ttu-id="3248c-124">带托管磁盘的虚拟机规模集</span><span class="sxs-lookup"><span data-stu-id="3248c-124">Virtual Machine Scale Sets with Managed Disks</span></span>

<span data-ttu-id="3248c-125">在托管磁盘推出之前，需针对要放入规模集的所有 VM 手动创建存储帐户，然后使用列表参数 ``vhd_containers`` 将所有存储帐户名称提供给规模集 RestAPI。</span><span class="sxs-lookup"><span data-stu-id="3248c-125">Before Managed Disks, you needed to create a storage account manually for all the VMs you wanted inside your Scale Set, and then use the list parameter ``vhd_containers`` to provide all the storage account name to the Scale Set RestAPI.</span></span> <span data-ttu-id="3248c-126">此文 `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>` 中提供了正式版转换指南。</span><span class="sxs-lookup"><span data-stu-id="3248c-126">The official transition guide is available in this article `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`.</span></span>

<span data-ttu-id="3248c-127">现在，有了托管磁盘，不需要管理任何存储帐户。</span><span class="sxs-lookup"><span data-stu-id="3248c-127">Now with Managed Disk, you don't have to manage any storage account at all.</span></span> <span data-ttu-id="3248c-128">如果熟悉 VMSS Python SDK 的话，``storage_profile`` 与创建 VM 时所用的配置文件完全相同：</span><span class="sxs-lookup"><span data-stu-id="3248c-128">If you're are used to the VMSS Python SDK, your ``storage_profile`` can now be exactly the same as the one used in VM creation:</span></span>

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

<span data-ttu-id="3248c-129">完整示例：</span><span class="sxs-lookup"><span data-stu-id="3248c-129">The full sample being:</span></span>

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

## <a name="other-operations-with-managed-disks"></a><span data-ttu-id="3248c-130">可对托管磁盘执行的其他操作</span><span class="sxs-lookup"><span data-stu-id="3248c-130">Other Operations with Managed Disks</span></span>

### <a name="resizing-a-managed-disk"></a><span data-ttu-id="3248c-131">调整托管磁盘的大小。</span><span class="sxs-lookup"><span data-stu-id="3248c-131">Resizing a managed disk.</span></span>

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

### <a name="update-the-storage-account-type-of-the-managed-disks"></a><span data-ttu-id="3248c-132">更新托管磁盘的存储帐户类型。</span><span class="sxs-lookup"><span data-stu-id="3248c-132">Update the Storage Account type of the Managed Disks.</span></span>
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

### <a name="create-an-image-from-blob-storage"></a><span data-ttu-id="3248c-133">从 Blob 存储创建映像。</span><span class="sxs-lookup"><span data-stu-id="3248c-133">Create an image from Blob Storage.</span></span>
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

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a><span data-ttu-id="3248c-134">创建当前已附加到虚拟机的托管磁盘的快照。</span><span class="sxs-lookup"><span data-stu-id="3248c-134">Create a snapshot of a Managed Disk that is currently attached to a Virtual Machine.</span></span>
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
