---
title: "使用用于 Python 的 Azure 管理库进行身份验证"
description: "在用于 Python 的 Azure 管理库中使用服务主体进行身份验证"
keywords: "Azure, Python, SDK, API, 身份验证, active directory, 服务主体"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5c4cf1dee7d9864e809f2797ad49ce78886a6f66
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2017
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="ff8c9-104">使用用于 Python 的 Azure 管理库进行身份验证</span><span class="sxs-lookup"><span data-stu-id="ff8c9-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="ff8c9-105">使用 Python 管理库创建和管理资源时，可借助多个选项在 Azure 中对应用程序进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="ff8c9-106">使用令牌凭据进行身份验证</span><span class="sxs-lookup"><span data-stu-id="ff8c9-106">Authenticate with token credentials</span></span>

<span data-ttu-id="ff8c9-107">请将凭据安全存储在配置文件、注册表或 Azure KeyVault 中。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="ff8c9-108">以下示例使用[服务主体](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="ff8c9-109">可以通过 Azure CLI 2.0 创建服务主体</span><span class="sxs-lookup"><span data-stu-id="ff8c9-109">You can create a Service Principal via the Azure CLI 2.0</span></span>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```

```python
    from azure.common.credentials import ServicePrincipalCredentials

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID
    )
```

> <span data-ttu-id="ff8c9-110">[NOTE!] 若要连接到 Azure 主权云之一，请使用 `cloud_environment` 参数。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-110">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>

```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID,
        cloud_environment = AZURE_CHINA_CLOUD
    )
```

<span data-ttu-id="ff8c9-111">如需更高的控制度，我们建议使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包装器。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-111">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="ff8c9-112">请参阅 ADAL 网站获取所有可用方案的列表和示例。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-112">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="ff8c9-113">服务主体身份验证的示例：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-113">For instance for service principal authentication:</span></span>

```python
    import adal
    from msrestazure.azure_active_directory import AdalAuthentication
    from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
    RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

    context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
    credentials = AdalAuthentication(
        context.acquire_token_with_client_credentials,
        RESOURCE,
        CLIENT,
        KEY
    )
```

<span data-ttu-id="ff8c9-114">可结合 `AdalAuthentication` 类使用所有 ADAL 有效调用。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-114">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="ff8c9-115">接下来，创建客户端对象来开始使用 API：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-115">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="ff8c9-116">[NOTE!] 如果使用 Azure 主权云，还必须在创建管理客户端时指定相应的基 URL（通过 `msrestazure.azure_cloud` 中的常量）。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-116">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="ff8c9-117">例如，对于 Azure 中国云：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-117">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="ff8c9-118">基于文件的身份验证</span><span class="sxs-lookup"><span data-stu-id="ff8c9-118">File based authentication</span></span>

<span data-ttu-id="ff8c9-119">最简单的身份验证方法是创建包含 Azure 服务主体凭据的 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-119">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="ff8c9-120">可使用以下 CLI 命令同时创建新的服务主体和此文件：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-120">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="ff8c9-121">将此文件保存在系统上可供代码读取的安全位置。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-121">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="ff8c9-122">在 shell 中将包含完整路径的环境变量设置为此文件：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-122">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="ff8c9-123">若要自行创建该文件，请采用以下格式：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-123">If you want to create the file yourself, please follow this format:</span></span>

```json
{
    "clientId": "ad735158-65ca-11e7-ba4d-ecb1d756380e",
    "clientSecret": "b70bb224-65ca-11e7-810c-ecb1d756380e",
    "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
    "tenantId": "c81da1d8-65ca-11e7-b1d1-ecb1d756380e",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

<span data-ttu-id="ff8c9-124">然后，可以使用客户端工厂创建任何客户端：</span><span class="sxs-lookup"><span data-stu-id="ff8c9-124">You can then create any client using the client factory:</span></span>
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="ff8c9-125">使用托管服务标识 (MSI) 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="ff8c9-125">Authenticate with Managed Service Identity(MSI)</span></span> 
<span data-ttu-id="ff8c9-126">MSI 是一种简单的方式，通过这种方式，Azure 中的资源无需创建特定凭据即可使用 SDK/CLI。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-126">MSI is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

    # Create MSI Authentication
    credentials = MSIAuthentication()


    # Create a Subscription Client
    subscription_client = SubscriptionClient(credentials)
    subscription = next(subscription_client.subscriptions.list())
    subscription_id = subscription.subscription_id

    # Create a Resource Management client
    resource_client = ResourceManagementClient(credentials, subscription_id)

    
    # List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
    for resource_group in resource_client.resource_groups.list():
        print(resource_group.name)

```

## <a name="mgmt-auth-cli"></a><span data-ttu-id="ff8c9-127">基于 CLI 的身份验证</span><span class="sxs-lookup"><span data-stu-id="ff8c9-127">CLI-based authentication</span></span>

<span data-ttu-id="ff8c9-128">SDK 能够使用 CLI 活动订阅创建客户端。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-128">The SDK is able to create a client using your CLI active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff8c9-129">应将此方法用作开发人员快速入门体验。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-129">This should be used as quick start developer experience.</span></span> <span data-ttu-id="ff8c9-130">对于生产用途，请使用 [ADAL](#authenticate-with-token-credentials) 或自己的凭据系统。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-130">For production purposes, use [ADAL](#authenticate-with-token-credentials) or your own credentials system.</span></span>
> <span data-ttu-id="ff8c9-131">对 CLI 配置进行任何更改会影响 SDK 的执行。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-131">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="ff8c9-132">若要定义活动的凭据，请使用 [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-132">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="ff8c9-133">默认的订阅 ID 是你拥有的唯一 ID，或使用 [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) 定义的 ID</span><span class="sxs-lookup"><span data-stu-id="ff8c9-133">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="ff8c9-134">使用令牌凭据进行身份验证（传统方法）</span><span class="sxs-lookup"><span data-stu-id="ff8c9-134">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="ff8c9-135">以前的 SDK 版本尚未推出 ADAL，而是提供一个 `UserPassCredentials` 类。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-135">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="ff8c9-136">此类被视为已弃用，不应继续使用。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-136">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="ff8c9-137">此示例演示用户/密码方案。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-137">This sample shows user/password scenario.</span></span> <span data-ttu-id="ff8c9-138">此方案不支持 2FA。</span><span class="sxs-lookup"><span data-stu-id="ff8c9-138">This does not support 2FA.</span></span>

```python
    from azure.common.credentials import UserPassCredentials

    credentials = UserPassCredentials(
        'user@domain.com',
        'my_smart_password',
    )
```
