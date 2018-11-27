---
title: Azure HDInsight Python SDK 预览版
description: Azure HDInsight Python SDK 参考。 HDInsight Python SDK 提供用于管理 HDInsight 群集的类和方法。
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 09/18/2018
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 42e1e36b5854fda93188564be3ed3064b9ba4435
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277465"
---
# <a name="hdinsight-python-management-sdk-preview"></a><span data-ttu-id="726da-104">HDInsight Python 管理 SDK 预览版</span><span class="sxs-lookup"><span data-stu-id="726da-104">HDInsight Python Management SDK Preview</span></span>

## <a name="overview"></a><span data-ttu-id="726da-105">概述</span><span class="sxs-lookup"><span data-stu-id="726da-105">Overview</span></span>

<span data-ttu-id="726da-106">HDInsight Python SDK 提供用于管理 HDInsight 群集的类和方法。</span><span class="sxs-lookup"><span data-stu-id="726da-106">The HDInsight Python SDK provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="726da-107">该 SDK 包含用于创建、删除、更新、列出、缩放、执行脚本操作，以及监视、获取 HDInsight 群集属性等的操作。</span><span class="sxs-lookup"><span data-stu-id="726da-107">It includes operations to create, delete, update, list, scale, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="726da-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="726da-108">Prerequisites</span></span>

* <span data-ttu-id="726da-109">一个 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="726da-109">An Azure account.</span></span> <span data-ttu-id="726da-110">如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="726da-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* [<span data-ttu-id="726da-111">Python</span><span class="sxs-lookup"><span data-stu-id="726da-111">Python</span></span>](https://www.python.org/downloads/)
* [<span data-ttu-id="726da-112">pip</span><span class="sxs-lookup"><span data-stu-id="726da-112">pip</span></span>](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a><span data-ttu-id="726da-113">SDK 安装</span><span class="sxs-lookup"><span data-stu-id="726da-113">SDK Installation</span></span>

<span data-ttu-id="726da-114">[Python 包索引](https://pypi.org/project/azure-mgmt-hdinsight/)中提供 HDInsight Python SDK，可以通过运行以下命令来安装它：</span><span class="sxs-lookup"><span data-stu-id="726da-114">The HDInsight Python SDK can be found in the [Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) and can be installed by running:</span></span> 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a><span data-ttu-id="726da-115">身份验证</span><span class="sxs-lookup"><span data-stu-id="726da-115">Authentication</span></span>

<span data-ttu-id="726da-116">首先需要使用 Azure 订阅对该 SDK 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="726da-116">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="726da-117">请遵循以下示例创建服务主体，然后使用该服务主体进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="726da-117">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="726da-118">完成此操作后，将会获得 `HDInsightManagementClient` 的实例，其中包含可用于执行管理操作的多个方法（以下部分将概述这些方法）。</span><span class="sxs-lookup"><span data-stu-id="726da-118">After this is done, you will have an instance of an `HDInsightManagementClient`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="726da-119">除了以下示例中所示的方法以外，还有其他一些身份验证方法可能更符合你的需要。</span><span class="sxs-lookup"><span data-stu-id="726da-119">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="726da-120">[使用用于 Python 的 Azure 管理库进行身份验证](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)中概述了所有方法</span><span class="sxs-lookup"><span data-stu-id="726da-120">All methods are outlined here: [Authenticate with the Azure Management Libraries for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="726da-121">使用服务主体的身份验证示例</span><span class="sxs-lookup"><span data-stu-id="726da-121">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="726da-122">首先登录到 [Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="726da-122">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="726da-123">验证当前使用的是要在其中创建服务主体的订阅。</span><span class="sxs-lookup"><span data-stu-id="726da-123">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="726da-124">订阅信息将显示为 JSON。</span><span class="sxs-lookup"><span data-stu-id="726da-124">Your subscription information is displayed as JSON.</span></span>

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

<span data-ttu-id="726da-125">如果尚未登录到正确的订阅，请运行以下命令选择正确的订阅：</span><span class="sxs-lookup"><span data-stu-id="726da-125">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="726da-126">如果尚未通过其他方法（例如，通过 Azure 门户创建 HDInsight 群集）注册 HDInsight 资源提供程序，则需要先执行此操作一次，然后才能进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="726da-126">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="726da-127">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来完成此操作：</span><span class="sxs-lookup"><span data-stu-id="726da-127">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="726da-128">接下来，选择服务主体的名称，然后使用以下命令创建服务主体：</span><span class="sxs-lookup"><span data-stu-id="726da-128">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="726da-129">服务主体信息将以 JSON 格式显示。</span><span class="sxs-lookup"><span data-stu-id="726da-129">The service principal information is displayed as JSON.</span></span>

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
<span data-ttu-id="726da-130">复制以下 Python 代码片段，并在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填写运行创建服务主体的命令后返回的 JSON 中的字符串。</span><span class="sxs-lookup"><span data-stu-id="726da-130">Copy the below Python snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

```python
from azure.mgmt.hdinsight import HDInsightManagementClient
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.hdinsight.models import *

# Tenant ID for your Azure Subscription
TENANT_ID = ''
# Your Service Principal App Client ID
CLIENT_ID = ''
# Your Service Principal Client Secret
CLIENT_SECRET = ''
# Your Azure Subscription ID
SUBSCRIPTION_ID = ''

credentials = ServicePrincipalCredentials(
    client_id = CLIENT_ID,
    secret = CLIENT_SECRET,
    tenant = TENANT_ID
)

client = HDInsightManagementClient(credentials, SUBSCRIPTION_ID)
```


## <a name="cluster-management"></a><span data-ttu-id="726da-131">群集管理</span><span class="sxs-lookup"><span data-stu-id="726da-131">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="726da-132">本部分假设你已完成身份验证，已构造 `HDInsightManagementClient` 实例并已将其存储在名为 `client` 的变量中。</span><span class="sxs-lookup"><span data-stu-id="726da-132">This section assumes you have already authenticated and constructed an `HDInsightManagementClient` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="726da-133">在前面的“身份验证”部分可以找到有关身份验证和获取 `HDInsightManagementClient` 的说明。</span><span class="sxs-lookup"><span data-stu-id="726da-133">Instructions for authenticating and obtaining an `HDInsightManagementClient` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="726da-134">创建群集</span><span class="sxs-lookup"><span data-stu-id="726da-134">Create a Cluster</span></span>

<span data-ttu-id="726da-135">可以通过调用 `client.clusters.create()` 来创建新群集。</span><span class="sxs-lookup"><span data-stu-id="726da-135">A new cluster can be created by calling `client.clusters.create()`.</span></span> 

#### <a name="example"></a><span data-ttu-id="726da-136">示例</span><span class="sxs-lookup"><span data-stu-id="726da-136">Example</span></span>

<span data-ttu-id="726da-137">本示例演示如何创建包含 2 个头节点和 1 个工作节点的 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="726da-137">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="726da-138">首先需要创建一个资源组和存储帐户，下面将予以介绍。</span><span class="sxs-lookup"><span data-stu-id="726da-138">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="726da-139">如果已创建资源组和存储帐户，则可以跳过这些步骤。</span><span class="sxs-lookup"><span data-stu-id="726da-139">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="726da-140">创建资源组</span><span class="sxs-lookup"><span data-stu-id="726da-140">Creating a Resource Group</span></span>

<span data-ttu-id="726da-141">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建资源组</span><span class="sxs-lookup"><span data-stu-id="726da-141">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="726da-142">创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="726da-142">Creating a Storage Account</span></span>

<span data-ttu-id="726da-143">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="726da-143">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="726da-144">现在，运行以下命令获取存储帐户的密钥（创建群集时需要用到）：</span><span class="sxs-lookup"><span data-stu-id="726da-144">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="726da-145">以下 Python 代码片段创建包含 2 个头节点和 1 个工作节点的 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="726da-145">The below Python snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="726da-146">按照注释中所述填写空白变量，并根据具体的需要任意更改其他参数。</span><span class="sxs-lookup"><span data-stu-id="726da-146">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

```python
# The name for the cluster you are creating
cluster_name = ""
# The name of your existing Resource Group
resource_group_name = ""
# Choose a username
username = ""
# Choose a password
password = ""
# Replace <> with the name of your storage account
storage_account = "<>.blob.core.windows.net"
# Storage account key you obtained above
storage_account_key = ""
# Choose a region
location = ""
container = "default"

params = ClusterCreateProperties(
    cluster_version="3.6",
    os_type=OSType.linux,
    tier=Tier.standard,
    cluster_definition=ClusterDefinition(
        kind="spark",
        configurations={
            "gateway": {
                "restAuthCredential.enabled_credential": "True",
                "restAuthCredential.username": username,
                "restAuthCredential.password": password
            }
        }
    ),
    compute_profile=ComputeProfile(
        roles=[
            Role(
                name="headnode",
                target_instance_count=2,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            ),
            Role(
                name="workernode",
                target_instance_count=1,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            )
        ]
    ),
    storage_profile=StorageProfile(
        storageaccounts=[StorageAccount(
            name=storage_account,
            key=storage_account_key,
            container=container,
            is_default=True
        )]
    )
)

client.clusters.create(
    cluster_name=cluster_name,
    resource_group_name=resource_group_name,
    parameters=ClusterCreateParametersExtended(
        location=location,
        tags={},
        properties=params
    ))
```

### <a name="get-cluster-details"></a><span data-ttu-id="726da-147">获取群集详细信息</span><span class="sxs-lookup"><span data-stu-id="726da-147">Get Cluster Details</span></span>

<span data-ttu-id="726da-148">获取给定群集的属性：</span><span class="sxs-lookup"><span data-stu-id="726da-148">To get properties for a given cluster:</span></span>

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="726da-149">示例</span><span class="sxs-lookup"><span data-stu-id="726da-149">Example</span></span>

<span data-ttu-id="726da-150">可以使用 `get` 来确认已成功创建群集。</span><span class="sxs-lookup"><span data-stu-id="726da-150">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

<span data-ttu-id="726da-151">输出应如下所示：</span><span class="sxs-lookup"><span data-stu-id="726da-151">The output should look like:</span></span>

```
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a><span data-ttu-id="726da-152">列出群集</span><span class="sxs-lookup"><span data-stu-id="726da-152">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="726da-153">列出订阅下的群集</span><span class="sxs-lookup"><span data-stu-id="726da-153">List Clusters Under The Subscription</span></span>

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="726da-154">按资源组列出群集</span><span class="sxs-lookup"><span data-stu-id="726da-154">List Clusters By Resource Group</span></span>

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> <span data-ttu-id="726da-155">`list()` 和 `list_by_resource_group()` 都返回 `ClusterPaged` 对象。</span><span class="sxs-lookup"><span data-stu-id="726da-155">Both `list()` and `list_by_resource_group()` return an `ClusterPaged` object.</span></span> <span data-ttu-id="726da-156">调用 `advance_page()` 会在该页上返回群集列表，并会将 `ClusterPaged` 对象推到下一页。</span><span class="sxs-lookup"><span data-stu-id="726da-156">Calling `advance_page()` returns the a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="726da-157">此操作可以一直重复，直至引发 `StopIteration` 异常（表明没有其他页）。</span><span class="sxs-lookup"><span data-stu-id="726da-157">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="726da-158">示例</span><span class="sxs-lookup"><span data-stu-id="726da-158">Example</span></span>

<span data-ttu-id="726da-159">以下示例列显当前订阅的所有群集的属性：</span><span class="sxs-lookup"><span data-stu-id="726da-159">The following example prints the properties of all clusters for the current subscription:</span></span>

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a><span data-ttu-id="726da-160">删除群集</span><span class="sxs-lookup"><span data-stu-id="726da-160">Delete a Cluster</span></span>

<span data-ttu-id="726da-161">删除群集：</span><span class="sxs-lookup"><span data-stu-id="726da-161">To delete a cluster:</span></span>

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a><span data-ttu-id="726da-162">更新群集标记</span><span class="sxs-lookup"><span data-stu-id="726da-162">Update Cluster Tags</span></span>

<span data-ttu-id="726da-163">可按如下所示更新给定群集的标记：</span><span class="sxs-lookup"><span data-stu-id="726da-163">You can update the tags of a given cluster like so:</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a><span data-ttu-id="726da-164">示例</span><span class="sxs-lookup"><span data-stu-id="726da-164">Example</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="scale-cluster"></a><span data-ttu-id="726da-165">缩放群集</span><span class="sxs-lookup"><span data-stu-id="726da-165">Scale Cluster</span></span>

<span data-ttu-id="726da-166">可以通过指定新大小来缩放给定群集的工作节点数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="726da-166">You can scale a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="726da-167">群集监视</span><span class="sxs-lookup"><span data-stu-id="726da-167">Cluster Monitoring</span></span>

<span data-ttu-id="726da-168">使用 HDInsight 管理 SDK 还可以通过 Operations Management Suite (OMS) 来管理群集的监视。</span><span class="sxs-lookup"><span data-stu-id="726da-168">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="726da-169">启用 OMS 监视</span><span class="sxs-lookup"><span data-stu-id="726da-169">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="726da-170">若要启用 OMS 监视，必须已有一个 Log Analytics 工作区。</span><span class="sxs-lookup"><span data-stu-id="726da-170">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="726da-171">如果尚未创建此工作区，可以参阅[在 Azure 门户中创建 Log Analytics 工作区](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)了解相关操作。</span><span class="sxs-lookup"><span data-stu-id="726da-171">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="726da-172">在群集上启用 OMS 监视：</span><span class="sxs-lookup"><span data-stu-id="726da-172">To enable OMS Monitoring on your cluster:</span></span>

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="726da-173">查看 OMS 监视状态</span><span class="sxs-lookup"><span data-stu-id="726da-173">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="726da-174">获取群集上的 OMS 状态：</span><span class="sxs-lookup"><span data-stu-id="726da-174">To get the status of OMS on your cluster:</span></span>

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="726da-175">禁用 OMS 监视</span><span class="sxs-lookup"><span data-stu-id="726da-175">Disable OMS Monitoring</span></span>

<span data-ttu-id="726da-176">在群集上禁用 OMS：</span><span class="sxs-lookup"><span data-stu-id="726da-176">To disable OMS on your cluster:</span></span>

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a><span data-ttu-id="726da-177">脚本操作</span><span class="sxs-lookup"><span data-stu-id="726da-177">Script Actions</span></span>

<span data-ttu-id="726da-178">HDInsight 提供一个称为“脚本操作”的配置方法，该方法可调用用于自定义群集的自定义脚本。</span><span class="sxs-lookup"><span data-stu-id="726da-178">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="726da-179">有关如何使用脚本操作的详细信息，请参阅[使用脚本操作自定义基于 Linux 的 HDInsight 群集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="726da-179">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="726da-180">执行脚本操作</span><span class="sxs-lookup"><span data-stu-id="726da-180">Execute Script Actions</span></span>
<span data-ttu-id="726da-181">若要在给定群集上执行脚本操作，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="726da-181">To execute script actions on a given cluster:</span></span>

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="726da-182">删除脚本操作</span><span class="sxs-lookup"><span data-stu-id="726da-182">Delete Script Action</span></span>

<span data-ttu-id="726da-183">删除给定群集上指定的持久化脚本操作：</span><span class="sxs-lookup"><span data-stu-id="726da-183">To delete a specified persisted script action on a given cluster:</span></span>

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="726da-184">列出持久化脚本操作</span><span class="sxs-lookup"><span data-stu-id="726da-184">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="726da-185">`list()` 和 `list_persisted_scripts()` 返回 `RuntimeScriptActionDetailPaged` 对象。</span><span class="sxs-lookup"><span data-stu-id="726da-185">`list()` and `list_persisted_scripts()` return a `RuntimeScriptActionDetailPaged` object.</span></span> <span data-ttu-id="726da-186">调用 `advance_page()` 会在该页上返回 `RuntimeScriptActionDetail` 列表，并会将 `RuntimeScriptActionDetailPaged` 对象推到下一页。</span><span class="sxs-lookup"><span data-stu-id="726da-186">Calling `advance_page()` returns the a list of `RuntimeScriptActionDetail` on that page and advances the `RuntimeScriptActionDetailPaged` object to the next page.</span></span> <span data-ttu-id="726da-187">此操作可以一直重复，直至引发 `StopIteration` 异常（表明没有其他页）。</span><span class="sxs-lookup"><span data-stu-id="726da-187">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span> <span data-ttu-id="726da-188">请参阅以下示例。</span><span class="sxs-lookup"><span data-stu-id="726da-188">See the example below.</span></span>

<span data-ttu-id="726da-189">列出指定群集的所有持久化脚本操作：</span><span class="sxs-lookup"><span data-stu-id="726da-189">To list all persisted script actions for the specified cluster:</span></span>
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="726da-190">示例</span><span class="sxs-lookup"><span data-stu-id="726da-190">Example</span></span>

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="726da-191">列出所有脚本的执行历史记录</span><span class="sxs-lookup"><span data-stu-id="726da-191">List All Scripts' Execution History</span></span>

<span data-ttu-id="726da-192">列出指定群集的所有脚本的执行历史记录：</span><span class="sxs-lookup"><span data-stu-id="726da-192">To list all scripts' execution history for the specified cluster:</span></span>

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="726da-193">示例</span><span class="sxs-lookup"><span data-stu-id="726da-193">Example</span></span>

<span data-ttu-id="726da-194">此示例列显以往所有脚本执行活动的所有详细信息。</span><span class="sxs-lookup"><span data-stu-id="726da-194">This example prints all the details for all past script executions.</span></span>

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
