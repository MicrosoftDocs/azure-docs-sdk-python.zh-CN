---
title: 用于 Python 的 Azure 容器实例库
description: 用于 Python 的 Azure 容器实例库参考
keywords: Azure, python, SDK, API, ACI, 容器, 实例
author: dlepow
manager: jeconnoc
ms.date: 04/15/2019
ms.author: danlep
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: container-instances
ms.openlocfilehash: 88df9443efb98bc5cec26c5eb4b01a4956141d40
ms.sourcegitcommit: 1b45953f168cbf36869c24c1741d70153b88b9fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59675932"
---
# <a name="azure-container-instances-libraries-for-python"></a>用于 Python 的 Azure 容器实例库

使用用于 Python 的 Microsoft Azure 容器实例库来创建和管理 Azure 容器实例。 请阅读 [Azure 容器实例概述](/azure/container-instances/container-instances-overview)，了解详细信息。

## <a name="management-apis"></a>管理 API

使用管理库可在 Azure 中创建和管理 Azure 容器实例。

通过 pip 安装管理包：

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a>示例源

若要在上下文中查看以下代码示例，可以在以下 GitHub 存储库中找到它们：

[Azure-Samples/aci-docs-sample-python](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a>Authentication

若要对 SDK 客户端进行身份验证（如以下示例中的 Azure 容器实例和资源管理器客户端），最容易的方式之一是使用[基于文件的身份验证](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file)。 基于文件的身份验证会查询 `AZURE_AUTH_LOCATION` 环境变量中是否存在凭据文件的路径。 若要使用基于文件的身份验证，请执行以下操作：

1. 使用 [Azure CLI](/cli/azure) 或 [Cloud Shell](https://shell.azure.com/) 创建凭据文件：

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   如果使用 [Cloud Shell](https://shell.azure.com/) 生成凭据文件，请将其内容复制到 Python 应用程序能够访问的本地文件中。

2. 将 `AZURE_AUTH_LOCATION` 环境变量设置为生成的凭据文件的完整路径。 例如（在 Bash 中）：

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

创建凭据文件并填充 `AZURE_AUTH_LOCATION` 环境变量以后，请使用 [client_factory][client_factory] 模块的 `get_client_from_auth_file` 方法初始化 [ResourceManagementClient][ResourceManagementClient] 和 [ContainerInstanceManagementClient][ContainerInstanceManagementClient] 对象。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]

若要更详细地了解用于 Azure 的 Python 管理库中提供的身份验证方法，请参阅[使用用于 Python 的 Azure 管理库进行身份验证](/python/azure/python-sdk-azure-authenticate)。

## <a name="create-container-group---single-container"></a>创建容器组 - 单个容器

此示例创建包含单个容器的容器组

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L141 "Create single-container group")]

## <a name="create-container-group---multiple-containers"></a>创建容器组 - 多个容器

此示例创建的容器组包含两个容器：应用程序容器和挎斗容器。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L144-L197 "Create multi-container group")]

## <a name="create-task-based-container-group"></a>创建基于任务的容器组

此示例创建的容器组包含单个基于任务的容器。 此示例演示多项功能：

* [命令行重写](/azure/container-instances/container-instances-restart-policy#command-line-override) - 指定自定义命令行，该命令行不同于在容器的 Dockerfile `CMD` 行中指定的命令行。 使用命令行重写，可以指定在容器启动时执行的自定义命令行，重写植入到容器中的默认命令行。 若要在容器启动时执行多个命令，则适用以下规范：

   如果要运行带有多个命令行参数的**单个命令**，例如 `echo FOO BAR`，必须以字符串列表的形式向[容器][Container]的 `command` 属性提供这些参数。 例如：

   `command = ['echo', 'FOO', 'BAR']`

   但是，如果要运行可能带有多个参数的**多个命令**，必须执行 shell，并将链接的命令作为参数传递。 例如，这样可以执行 `echo` 和 `tail` 命令：

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* [环境变量](/azure/container-instances/container-instances-environment-variables) - 为容器组中的容器指定两个环境变量。 可以使用环境变量修改运行时的脚本或应用程序行为，或以其他方式向容器中运行的应用程序传递动态信息。
* [重启策略](/azure/container-instances/container-instances-restart-policy) - 为容器配置的重启策略为“从不”，这适用于基于任务的容器，此类容器是在执行批处理作业的过程中执行的。
* 使用 [AzureOperationPoller][AzureOperationPoller] 进行操作轮询 - 在调用 create 方法以后，会进行操作轮询以确定操作完成时间，并可获取容器组的日志。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L200-L276 "Run a task-based container")]

## <a name="list-container-groups"></a>列出容器组

此示例列出资源组中的容器组，然后列显其部分属性。

列出容器组时，每个返回的组的 [instance_view][instance_view] 为 `None`。 若要获取某个容器组中容器的详细信息，必须接着对该容器组执行 [get][containergroupoperations_get] 操作，以便返回 `instance_view` 属性已填充的组。 如需通过示例来了解如何在容器组的 `instance_view` 中循环访问该组的容器，请参阅下一部分：[获取现有的容器组](#get-an-existing-container-group)。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L279-L293 "List container groups")]

## <a name="get-an-existing-container-group"></a>获取现有的容器组

此示例获取资源组中的特定容器组，然后列显其部分属性（包括其容器）及属性值。

[get 操作][containergroupoperations_get]返回其 [instance_view][instance_view] 已填充的容器组，这样就可以循环访问组中的每个容器。 只有 `get` 操作会填充容器组的 `instance_vew` 属性--列出订阅或资源组中的容器组不会填充实例视图，因为列出操作的开销可能很昂贵（例如，如果列出数百个容器组，而每个容器组又可能包含多个容器，则开销会很大）。 如前面的[列出容器组](#list-container-groups)部分所述，在 `list` 操作之后，接着必须对特定的容器组执行 `get` 操作，以便获取其容器实例详细信息。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L296-L325 "Get container group")]

## <a name="delete-a-container-group"></a>删除容器组

此示例删除资源组中的多个容器组，以及该资源组本身。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]

## <a name="next-steps"></a>后续步骤

* 可以在 GitHub 上找到前述示例的源代码：

  [Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]

* 更多 Azure 容器实例代码示例：

  [Azure 代码示例][samples-aci]

* 了解更多可在应用中使用的[示例 Python 代码][samples-python]。

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/containerinstance/management)

<!-- LINKS - External -->
[aci-docs-sample-python]: https://github.com/Azure-Samples/aci-docs-sample-python
[samples-aci]: https://azure.microsoft.com/resources/samples/?sort=0&term=ACI
[samples-python]: https://azure.microsoft.com/resources/samples/?platform=python

<!-- TYPES -->
[AzureOperationPoller]: /python/api/msrestazure.azure_operation.AzureOperationPoller
[client_factory]: /python/api/azure.common.client_factory
[Container]: /python/api/azure.mgmt.containerinstance.models.container
[ContainerGroupInstanceView]: /python/api/azure.mgmt.containerinstance.models.containergrouppropertiesinstanceview
[containergroupoperations_get]: /python/api/azure.mgmt.containerinstance.operations.containergroupsoperations#get
[ContainerInstanceManagementClient]: /python/api/azure.mgmt.containerinstance.containerinstancemanagementclient
[instance_view]: /python/api/azure.mgmt.containerinstance.models.containergroup#variables
[ResourceManagementClient]: /python/api/azure.mgmt.resource.resources.resourcemanagementclient