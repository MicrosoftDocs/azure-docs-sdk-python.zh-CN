---
title: "用于 Python 的 Azure 资源库"
description: 
keywords: "Azure, Python, SDK, API, 资源"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: 32e13bee27db091f0bca12c7d9ae4fc62165f4c0
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="2c868-103">用于 Python 的 Azure 资源库</span><span class="sxs-lookup"><span data-stu-id="2c868-103">Azure Resources libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="2c868-104">概述</span><span class="sxs-lookup"><span data-stu-id="2c868-104">Overview</span></span> 
<span data-ttu-id="2c868-105">在资源组中管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="2c868-105">Manage Azure resources in resource groups.</span></span>

| <span data-ttu-id="2c868-106">程序包</span><span class="sxs-lookup"><span data-stu-id="2c868-106">Package</span></span>  |  <span data-ttu-id="2c868-107">说明</span><span class="sxs-lookup"><span data-stu-id="2c868-107">Description</span></span> |
|---|---|
|<span data-ttu-id="2c868-108">[azure.mgmt.resource.features][1]</span><span class="sxs-lookup"><span data-stu-id="2c868-108">[azure.mgmt.resource.features][1]</span></span>|<span data-ttu-id="2c868-109">Azure 功能公开控制 (AFEC) 提供一个机制让资源提供程序控制公开给用户的功能。</span><span class="sxs-lookup"><span data-stu-id="2c868-109">Azure Feature Exposure Control (AFEC) provides a mechanism for the resource providers to control feature exposure to users.</span></span>|
|<span data-ttu-id="2c868-110">[azure.mgmt.resource.links][2]</span><span class="sxs-lookup"><span data-stu-id="2c868-110">[azure.mgmt.resource.links][2]</span></span>|<span data-ttu-id="2c868-111">Azure 资源可以链接在一起，以构成逻辑关系。</span><span class="sxs-lookup"><span data-stu-id="2c868-111">Azure resources can be linked together to form logical relationships.</span></span> <span data-ttu-id="2c868-112">可以在属于不同资源组的资源之间建立链接。</span><span class="sxs-lookup"><span data-stu-id="2c868-112">You can establish links between resources belonging to different resource groups.</span></span>|
|<span data-ttu-id="2c868-113">[azure.mgmt.resource.locks][3]</span><span class="sxs-lookup"><span data-stu-id="2c868-113">[azure.mgmt.resource.locks][3]</span></span>|<span data-ttu-id="2c868-114">可以锁定 Azure 资源，防止组织中的其他用户删除或修改资源。</span><span class="sxs-lookup"><span data-stu-id="2c868-114">Azure resources can be locked to prevent other users in your organization from deleting or modifying resources.</span></span>|
|<span data-ttu-id="2c868-115">[azure.mgmt.resource.managedapplications][4]</span><span class="sxs-lookup"><span data-stu-id="2c868-115">[azure.mgmt.resource.managedapplications][4]</span></span>|<span data-ttu-id="2c868-116">ARM 托管应用程序（设备）。</span><span class="sxs-lookup"><span data-stu-id="2c868-116">ARM managed applications (appliances).</span></span>|
|<span data-ttu-id="2c868-117">[azure.mgmt.resource.policy][5]</span><span class="sxs-lookup"><span data-stu-id="2c868-117">[azure.mgmt.resource.policy][5]</span></span>|<span data-ttu-id="2c868-118">若要管理和控制对资源的访问，可以定义并在某个范围内分配自定义的策略。</span><span class="sxs-lookup"><span data-stu-id="2c868-118">To manage and control access to your resources, you can define customized policies and assign them at a scope.</span></span>|
|<span data-ttu-id="2c868-119">[azure.mgmt.resource.resources][6]</span><span class="sxs-lookup"><span data-stu-id="2c868-119">[azure.mgmt.resource.resources][6]</span></span>| <span data-ttu-id="2c868-120">提供用于处理资源和资源组的操作。</span><span class="sxs-lookup"><span data-stu-id="2c868-120">Provides operations for working with resources and resource groups.</span></span>|
|<span data-ttu-id="2c868-121">[azure.mgmt.resource.subscriptions][7]</span><span class="sxs-lookup"><span data-stu-id="2c868-121">[azure.mgmt.resource.subscriptions][7]</span></span>|<span data-ttu-id="2c868-122">所有资源组和资源都在订阅中。</span><span class="sxs-lookup"><span data-stu-id="2c868-122">All resource groups and resources exist within subscriptions.</span></span> <span data-ttu-id="2c868-123">使用这些操作可以获取有关订阅和租户的信息。</span><span class="sxs-lookup"><span data-stu-id="2c868-123">These operation enable you get information about your subscriptions and tenants.</span></span>|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a><span data-ttu-id="2c868-124">安装库</span><span class="sxs-lookup"><span data-stu-id="2c868-124">Install the libraries</span></span> 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="2c868-125">示例</span><span class="sxs-lookup"><span data-stu-id="2c868-125">Example</span></span>
<span data-ttu-id="2c868-126">以下示例演示如何创建资源组。</span><span class="sxs-lookup"><span data-stu-id="2c868-126">The following is an example of how to create a resource group.</span></span> 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

<span data-ttu-id="2c868-127">详细了解可在应用中使用的[示例 Python 代码](https://azure.microsoft.com/resources/samples/?platform=python)。</span><span class="sxs-lookup"><span data-stu-id="2c868-127">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span> 