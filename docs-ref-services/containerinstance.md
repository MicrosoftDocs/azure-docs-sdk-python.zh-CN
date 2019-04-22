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
# <a name="azure-container-instances-libraries-for-python"></a><span data-ttu-id="f602a-104">用于 Python 的 Azure 容器实例库</span><span class="sxs-lookup"><span data-stu-id="f602a-104">Azure Container Instances libraries for Python</span></span>

<span data-ttu-id="f602a-105">使用用于 Python 的 Microsoft Azure 容器实例库来创建和管理 Azure 容器实例。</span><span class="sxs-lookup"><span data-stu-id="f602a-105">Use the Microsoft Azure Container Instances libraries for Python to create and manage Azure container instances.</span></span> <span data-ttu-id="f602a-106">请阅读 [Azure 容器实例概述](/azure/container-instances/container-instances-overview)，了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="f602a-106">Learn more by reading the [Azure Container Instances overview](/azure/container-instances/container-instances-overview).</span></span>

## <a name="management-apis"></a><span data-ttu-id="f602a-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="f602a-107">Management APIs</span></span>

<span data-ttu-id="f602a-108">使用管理库可在 Azure 中创建和管理 Azure 容器实例。</span><span class="sxs-lookup"><span data-stu-id="f602a-108">Use the management library to create and manage Azure container instances in Azure.</span></span>

<span data-ttu-id="f602a-109">通过 pip 安装管理包：</span><span class="sxs-lookup"><span data-stu-id="f602a-109">Install the management package via pip:</span></span>

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a><span data-ttu-id="f602a-110">示例源</span><span class="sxs-lookup"><span data-stu-id="f602a-110">Example source</span></span>

<span data-ttu-id="f602a-111">若要在上下文中查看以下代码示例，可以在以下 GitHub 存储库中找到它们：</span><span class="sxs-lookup"><span data-stu-id="f602a-111">If you'd like to see the following code examples in context, you can find them in the following GitHub repository:</span></span>

[<span data-ttu-id="f602a-112">Azure-Samples/aci-docs-sample-python</span><span class="sxs-lookup"><span data-stu-id="f602a-112">Azure-Samples/aci-docs-sample-python</span></span>](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a><span data-ttu-id="f602a-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="f602a-113">Authentication</span></span>

<span data-ttu-id="f602a-114">若要对 SDK 客户端进行身份验证（如以下示例中的 Azure 容器实例和资源管理器客户端），最容易的方式之一是使用[基于文件的身份验证](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file)。</span><span class="sxs-lookup"><span data-stu-id="f602a-114">One of the easiest ways to authenticate SDK clients (like the Azure Container Instances and Resource Manager clients in the following example) is with [file-based authentication](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file).</span></span> <span data-ttu-id="f602a-115">基于文件的身份验证会查询 `AZURE_AUTH_LOCATION` 环境变量中是否存在凭据文件的路径。</span><span class="sxs-lookup"><span data-stu-id="f602a-115">File-based authentication queries the `AZURE_AUTH_LOCATION` environment variable for the path to a credentials file.</span></span> <span data-ttu-id="f602a-116">若要使用基于文件的身份验证，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="f602a-116">To use file-based authentication:</span></span>

1. <span data-ttu-id="f602a-117">使用 [Azure CLI](/cli/azure) 或 [Cloud Shell](https://shell.azure.com/) 创建凭据文件：</span><span class="sxs-lookup"><span data-stu-id="f602a-117">Create a credentials file with the [Azure CLI](/cli/azure) or [Cloud Shell](https://shell.azure.com/):</span></span>

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   <span data-ttu-id="f602a-118">如果使用 [Cloud Shell](https://shell.azure.com/) 生成凭据文件，请将其内容复制到 Python 应用程序能够访问的本地文件中。</span><span class="sxs-lookup"><span data-stu-id="f602a-118">If you use the [Cloud Shell](https://shell.azure.com/) to generate the credentials file, copy its contents into a local file that your Python application can access.</span></span>

2. <span data-ttu-id="f602a-119">将 `AZURE_AUTH_LOCATION` 环境变量设置为生成的凭据文件的完整路径。</span><span class="sxs-lookup"><span data-stu-id="f602a-119">Set the `AZURE_AUTH_LOCATION` environment variable to the full path of the generated credentials file.</span></span> <span data-ttu-id="f602a-120">例如（在 Bash 中）：</span><span class="sxs-lookup"><span data-stu-id="f602a-120">For example (in Bash):</span></span>

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

<span data-ttu-id="f602a-121">创建凭据文件并填充 `AZURE_AUTH_LOCATION` 环境变量以后，请使用 [client_factory][client_factory] 模块的 `get_client_from_auth_file` 方法初始化 [ResourceManagementClient][ResourceManagementClient] 和 [ContainerInstanceManagementClient][ContainerInstanceManagementClient] 对象。</span><span class="sxs-lookup"><span data-stu-id="f602a-121">Once you've created the credentials file and populated the `AZURE_AUTH_LOCATION` environment variable, use the `get_client_from_auth_file` method of the [client_factory][client_factory] module to initialize the [ResourceManagementClient][ResourceManagementClient] and [ContainerInstanceManagementClient][ContainerInstanceManagementClient] objects.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]

<span data-ttu-id="f602a-122">若要更详细地了解用于 Azure 的 Python 管理库中提供的身份验证方法，请参阅[使用用于 Python 的 Azure 管理库进行身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="f602a-122">For more details about the available authentication methods in the Python management libraries for Azure, see [Authenticate with the Azure Management Libraries for Python](/python/azure/python-sdk-azure-authenticate).</span></span>

## <a name="create-container-group---single-container"></a><span data-ttu-id="f602a-123">创建容器组 - 单个容器</span><span class="sxs-lookup"><span data-stu-id="f602a-123">Create container group - single container</span></span>

<span data-ttu-id="f602a-124">此示例创建包含单个容器的容器组</span><span class="sxs-lookup"><span data-stu-id="f602a-124">This example creates a container group with a single container</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L141 "Create single-container group")]

## <a name="create-container-group---multiple-containers"></a><span data-ttu-id="f602a-125">创建容器组 - 多个容器</span><span class="sxs-lookup"><span data-stu-id="f602a-125">Create container group - multiple containers</span></span>

<span data-ttu-id="f602a-126">此示例创建的容器组包含两个容器：应用程序容器和挎斗容器。</span><span class="sxs-lookup"><span data-stu-id="f602a-126">This example creates a container group with two containers: an application container and a sidecar container.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L144-L197 "Create multi-container group")]

## <a name="create-task-based-container-group"></a><span data-ttu-id="f602a-127">创建基于任务的容器组</span><span class="sxs-lookup"><span data-stu-id="f602a-127">Create task-based container group</span></span>

<span data-ttu-id="f602a-128">此示例创建的容器组包含单个基于任务的容器。</span><span class="sxs-lookup"><span data-stu-id="f602a-128">This example creates a container group with a single task-based container.</span></span> <span data-ttu-id="f602a-129">此示例演示多项功能：</span><span class="sxs-lookup"><span data-stu-id="f602a-129">This example demonstrates several features:</span></span>

* <span data-ttu-id="f602a-130">[命令行重写](/azure/container-instances/container-instances-restart-policy#command-line-override) - 指定自定义命令行，该命令行不同于在容器的 Dockerfile `CMD` 行中指定的命令行。</span><span class="sxs-lookup"><span data-stu-id="f602a-130">[Command line override](/azure/container-instances/container-instances-restart-policy#command-line-override) - A custom command line, different from that which is specified in the container's Dockerfile `CMD` line, is specified.</span></span> <span data-ttu-id="f602a-131">使用命令行重写，可以指定在容器启动时执行的自定义命令行，重写植入到容器中的默认命令行。</span><span class="sxs-lookup"><span data-stu-id="f602a-131">Command line override allows you to specify a custom command line to execute at container startup, overriding the default command line baked-in to the container.</span></span> <span data-ttu-id="f602a-132">若要在容器启动时执行多个命令，则适用以下规范：</span><span class="sxs-lookup"><span data-stu-id="f602a-132">Regarding executing multiple commands at container startup, the following applies:</span></span>

   <span data-ttu-id="f602a-133">如果要运行带有多个命令行参数的**单个命令**，例如 `echo FOO BAR`，必须以字符串列表的形式向[容器][Container]的 `command` 属性提供这些参数。</span><span class="sxs-lookup"><span data-stu-id="f602a-133">If you want to run a **single command** with several command-line arguments, for example `echo FOO BAR`, you must supply them as a string list to the `command` property of the [Container][Container].</span></span> <span data-ttu-id="f602a-134">例如：</span><span class="sxs-lookup"><span data-stu-id="f602a-134">For example:</span></span>

   `command = ['echo', 'FOO', 'BAR']`

   <span data-ttu-id="f602a-135">但是，如果要运行可能带有多个参数的**多个命令**，必须执行 shell，并将链接的命令作为参数传递。</span><span class="sxs-lookup"><span data-stu-id="f602a-135">If, however, you want to run **multiple commands** with (potentially) multiple arguments, you must execute a shell and pass the chained commands as an argument.</span></span> <span data-ttu-id="f602a-136">例如，这样可以执行 `echo` 和 `tail` 命令：</span><span class="sxs-lookup"><span data-stu-id="f602a-136">For example, this executes both an `echo` and a `tail` command:</span></span>

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* <span data-ttu-id="f602a-137">[环境变量](/azure/container-instances/container-instances-environment-variables) - 为容器组中的容器指定两个环境变量。</span><span class="sxs-lookup"><span data-stu-id="f602a-137">[Environment variables](/azure/container-instances/container-instances-environment-variables) - Two environment variables are specified for the container in the container group.</span></span> <span data-ttu-id="f602a-138">可以使用环境变量修改运行时的脚本或应用程序行为，或以其他方式向容器中运行的应用程序传递动态信息。</span><span class="sxs-lookup"><span data-stu-id="f602a-138">Use environment variables to modify script or application behavior at runtime, or otherwise pass dynamic information to an application running in the container.</span></span>
* <span data-ttu-id="f602a-139">[重启策略](/azure/container-instances/container-instances-restart-policy) - 为容器配置的重启策略为“从不”，这适用于基于任务的容器，此类容器是在执行批处理作业的过程中执行的。</span><span class="sxs-lookup"><span data-stu-id="f602a-139">[Restart policy](/azure/container-instances/container-instances-restart-policy) - The container is configured with a restart policy of "Never," useful for task-based containers that are executed as part of a batch job.</span></span>
* <span data-ttu-id="f602a-140">使用 [AzureOperationPoller][AzureOperationPoller] 进行操作轮询 - 在调用 create 方法以后，会进行操作轮询以确定操作完成时间，并可获取容器组的日志。</span><span class="sxs-lookup"><span data-stu-id="f602a-140">Operation polling with [AzureOperationPoller][AzureOperationPoller] - After the create method is invoked, the operation is polled to determine when it has completed and the container group's logs can be obtained.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L200-L276 "Run a task-based container")]

## <a name="list-container-groups"></a><span data-ttu-id="f602a-141">列出容器组</span><span class="sxs-lookup"><span data-stu-id="f602a-141">List container groups</span></span>

<span data-ttu-id="f602a-142">此示例列出资源组中的容器组，然后列显其部分属性。</span><span class="sxs-lookup"><span data-stu-id="f602a-142">This example lists the container groups in a resource group and then prints a few of their properties.</span></span>

<span data-ttu-id="f602a-143">列出容器组时，每个返回的组的 [instance_view][instance_view] 为 `None`。</span><span class="sxs-lookup"><span data-stu-id="f602a-143">When you list container groups,the [instance_view][instance_view] of each returned group is `None`.</span></span> <span data-ttu-id="f602a-144">若要获取某个容器组中容器的详细信息，必须接着对该容器组执行 [get][containergroupoperations_get] 操作，以便返回 `instance_view` 属性已填充的组。</span><span class="sxs-lookup"><span data-stu-id="f602a-144">To get the details of the containers within a container group, you must then [get][containergroupoperations_get] the container group, which returns the group with its `instance_view` property populated.</span></span> <span data-ttu-id="f602a-145">如需通过示例来了解如何在容器组的 `instance_view` 中循环访问该组的容器，请参阅下一部分：[获取现有的容器组](#get-an-existing-container-group)。</span><span class="sxs-lookup"><span data-stu-id="f602a-145">See the next section, [Get an existing container group](#get-an-existing-container-group), for an example of iterating over a container group's containers in its `instance_view`.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L279-L293 "List container groups")]

## <a name="get-an-existing-container-group"></a><span data-ttu-id="f602a-146">获取现有的容器组</span><span class="sxs-lookup"><span data-stu-id="f602a-146">Get an existing container group</span></span>

<span data-ttu-id="f602a-147">此示例获取资源组中的特定容器组，然后列显其部分属性（包括其容器）及属性值。</span><span class="sxs-lookup"><span data-stu-id="f602a-147">This example gets a specific container group from a resource group, and then prints a few of its properties (including its containers) and their values.</span></span>

<span data-ttu-id="f602a-148">[get 操作][containergroupoperations_get]返回其 [instance_view][instance_view] 已填充的容器组，这样就可以循环访问组中的每个容器。</span><span class="sxs-lookup"><span data-stu-id="f602a-148">The [get operation][containergroupoperations_get] returns a container group with its [instance_view][instance_view] populated, which allows you to iterate over each container in the group.</span></span> <span data-ttu-id="f602a-149">只有 `get` 操作会填充容器组的 `instance_vew` 属性--列出订阅或资源组中的容器组不会填充实例视图，因为列出操作的开销可能很昂贵（例如，如果列出数百个容器组，而每个容器组又可能包含多个容器，则开销会很大）。</span><span class="sxs-lookup"><span data-stu-id="f602a-149">Only the `get` operation populates the `instance_vew` property of the container group--listing the container groups in a subscription or resource group doesn't populate the instance view due to the potentially expensive nature of the operation (for example, when listing hundreds of container groups, each potentially containing multiple containers).</span></span> <span data-ttu-id="f602a-150">如前面的[列出容器组](#list-container-groups)部分所述，在 `list` 操作之后，接着必须对特定的容器组执行 `get` 操作，以便获取其容器实例详细信息。</span><span class="sxs-lookup"><span data-stu-id="f602a-150">As mentioned previously in the [List container groups](#list-container-groups) section, after a `list`, you must subsequently `get` a specific container group to obtain its container instance details.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L296-L325 "Get container group")]

## <a name="delete-a-container-group"></a><span data-ttu-id="f602a-151">删除容器组</span><span class="sxs-lookup"><span data-stu-id="f602a-151">Delete a container group</span></span>

<span data-ttu-id="f602a-152">此示例删除资源组中的多个容器组，以及该资源组本身。</span><span class="sxs-lookup"><span data-stu-id="f602a-152">This example deletes several container groups from a resource group, as well as the resource group.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]

## <a name="next-steps"></a><span data-ttu-id="f602a-153">后续步骤</span><span class="sxs-lookup"><span data-stu-id="f602a-153">Next steps</span></span>

* <span data-ttu-id="f602a-154">可以在 GitHub 上找到前述示例的源代码：</span><span class="sxs-lookup"><span data-stu-id="f602a-154">The source code for the preceding examples can be found on GitHub:</span></span>

  <span data-ttu-id="f602a-155">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span><span class="sxs-lookup"><span data-stu-id="f602a-155">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span></span>

* <span data-ttu-id="f602a-156">更多 Azure 容器实例代码示例：</span><span class="sxs-lookup"><span data-stu-id="f602a-156">More Azure Container Instances code samples:</span></span>

  <span data-ttu-id="f602a-157">[Azure 代码示例][samples-aci]</span><span class="sxs-lookup"><span data-stu-id="f602a-157">[Azure Code Samples][samples-aci]</span></span>

* <span data-ttu-id="f602a-158">了解更多可在应用中使用的[示例 Python 代码][samples-python]。</span><span class="sxs-lookup"><span data-stu-id="f602a-158">Explore more [sample Python code][samples-python] you can use in your apps.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f602a-159">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="f602a-159">Explore the management APIs</span></span>](/python/api/overview/azure/containerinstance/management)

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