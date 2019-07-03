---
title: Azure HDInsight SDK for Python
description: Azure HDInsight SDK for Python 参考。 HDInsight SDK for Python 提供用于管理 HDInsight 群集的类和方法。
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 04/10/2019
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 3b0799dd77f7ff447ef997b2d142a6744c4a6858
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534265"
---
# <a name="hdinsight-sdk-for-python"></a>HDInsight SDK for Python

## <a name="overview"></a>概述

HDInsight SDK for Python 提供用于管理 HDInsight 群集的类和方法。 该 SDK 包含用于创建、删除、更新、列出、调整大小、执行脚本操作，以及监视、获取 HDInsight 群集属性等操作。

## <a name="prerequisites"></a>先决条件

* 一个 Azure 帐户。 如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。
* [Python](https://www.python.org/downloads/)
* [pip](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a>SDK 安装

HDInsight SDK for Python 在 [Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) 中提供，可以通过运行以下命令来安装： 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a>Authentication

首先需要使用 Azure 订阅对该 SDK 进行身份验证。  请遵循以下示例创建服务主体，然后使用该服务主体进行身份验证。 完成此操作后，将会获得 `HDInsightManagementClient` 的实例，其中包含可用于执行管理操作的多个方法（以下部分将概述这些方法）。

> [!NOTE]
> 除了以下示例中所示的方法以外，还有其他一些身份验证方法可能更符合你的需要。 此处概述了所有方法：[使用用于 Python 的 Azure 管理库进行身份验证](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)

### <a name="authentication-example-using-a-service-principal"></a>使用服务主体的身份验证示例

首先登录到 [Azure Cloud Shell](https://shell.azure.com/bash)。 验证当前使用的是要在其中创建服务主体的订阅。 

```azurecli-interactive
az account show
```

订阅信息将显示为 JSON。

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

如果尚未登录到正确的订阅，请运行以下命令选择正确的订阅： 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> 如果尚未通过其他方法（例如，通过 Azure 门户创建 HDInsight 群集）注册 HDInsight 资源提供程序，则需要先执行此操作一次，然后才能进行身份验证。 可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来完成此操作：
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

接下来，选择服务主体的名称，然后使用以下命令创建服务主体：

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

服务主体信息将以 JSON 格式显示。

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
复制以下 Python 代码片段，并在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填写运行创建服务主体的命令后返回的 JSON 中的字符串。

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


## <a name="cluster-management"></a>群集管理

> [!NOTE]
> 本部分假设你已完成身份验证，已构造 `HDInsightManagementClient` 实例并已将其存储在名为 `client` 的变量中。 在前面的“身份验证”部分可以找到有关身份验证和获取 `HDInsightManagementClient` 的说明。

### <a name="create-a-cluster"></a>创建群集

可以通过调用 `client.clusters.create()` 来创建新群集。

#### <a name="samples"></a>示例

提供了用于创建几个常见类型的 HDInsight 群集的代码示例：[HDInsight Python 示例](https://github.com/Azure-Samples/hdinsight-python-sdk-samples)。

#### <a name="example"></a>示例

本示例演示如何创建包含 2 个头节点和 1 个工作节点的 Spark 群集。

> [!NOTE]
> 首先需要创建一个资源组和存储帐户，下面将予以介绍。 如果已创建资源组和存储帐户，则可以跳过这些步骤。

##### <a name="creating-a-resource-group"></a>创建资源组

可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建资源组
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>创建存储帐户

可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建存储帐户
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
现在，运行以下命令获取存储帐户的密钥（创建群集时需要用到）：
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
以下 Python 代码片段创建包含 2 个头节点和 1 个工作节点的 Spark 群集。 按照注释中所述填写空白变量，并根据具体的需要任意更改其他参数。

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

### <a name="get-cluster-details"></a>获取群集详细信息

获取给定群集的属性：

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>示例

可以使用 `get` 来确认已成功创建群集。

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

输出应如下所示：

```output
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a>列出群集

#### <a name="list-clusters-under-the-subscription"></a>列出订阅下的群集

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a>按资源组列出群集

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> `list()` 和 `list_by_resource_group()` 都返回 `ClusterPaged` 对象。 调用 `advance_page()` 会在该页上返回群集列表，并会将 `ClusterPaged` 对象推到下一页。 此操作可以一直重复，直至引发 `StopIteration` 异常（表明没有其他页）。

#### <a name="example"></a>示例

以下示例列显当前订阅的所有群集的属性：

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a>删除群集

删除群集：

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>更新群集标记

可按如下所示更新给定群集的标记：

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a>示例

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="resize-cluster"></a>调整群集大小

可以通过指定新大小来调整给定群集的工作节点数，如下所示：

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a>群集监视

使用 HDInsight 管理 SDK 还可以通过 Operations Management Suite (OMS) 来管理群集的监视。

### <a name="enable-oms-monitoring"></a>启用 OMS 监视

> [!NOTE]
> 若要启用 OMS 监视，必须已有一个 Log Analytics 工作区。 如果尚未创建工作区，可在此了解创建方法：[在 Azure 门户中创建 Log Analytics 工作区](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)。

在群集上启用 OMS 监视：

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a>查看 OMS 监视状态

获取群集上的 OMS 状态：

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>禁用 OMS 监视

在群集上禁用 OMS：

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a>脚本操作

HDInsight 提供一个称为“脚本操作”的配置方法，该方法可调用用于自定义群集的自定义脚本。
> [!NOTE]
> 有关如何使用脚本操作的详细信息见此处：[使用脚本操作自定义基于 Linux 的 HDInsight 群集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>执行脚本操作
若要在给定群集上执行脚本操作，请使用以下命令：

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a>删除脚本操作

删除给定群集上指定的持久化脚本操作：

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>列出持久化脚本操作

> [!NOTE]
> `list()` 和 `list_persisted_scripts()` 返回 `RuntimeScriptActionDetailPaged` 对象。 调用 `advance_page()` 会在该页上返回 `RuntimeScriptActionDetail` 列表，并会将 `RuntimeScriptActionDetailPaged` 对象推到下一页。 此操作可以一直重复，直至引发 `StopIteration` 异常（表明没有其他页）。 请参阅以下示例。

列出指定群集的所有持久化脚本操作：
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>示例

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a>列出所有脚本的执行历史记录

列出指定群集的所有脚本的执行历史记录：

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>示例

此示例列显以往所有脚本执行活动的所有详细信息。

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
