---
title: 使用用于 Python 的 Azure 管理库进行身份验证
description: 在用于 Python 的 Azure 管理库中使用服务主体进行身份验证
keywords: Azure, Python, SDK, API, 身份验证, active directory, 服务主体
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/11/2019
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 51f26b120cefffd2d7f4af9c2b6b2cb532bc6006
ms.sourcegitcommit: 375a1f9180eb1323fe2af0a7e28fd4676973c68e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2019
ms.locfileid: "59586805"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>使用用于 Python 的 Azure 管理库进行身份验证

使用 Python 管理库创建和管理资源时，可借助多个选项在 Azure 中对应用程序进行身份验证。

## <a name="mgmt-auth-token"></a>使用令牌凭据进行身份验证

请将凭据安全存储在配置文件、注册表或 Azure KeyVault 中。

以下示例使用[服务主体](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)进行身份验证。

> [!NOTE]
> 若要通过 Azure CLI 创建服务主体，请使用以下命令：
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> 若要详细了解如何通过 CLI 设置服务主体，请参阅[使用 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)

```python
from azure.common.credentials import ServicePrincipalCredentials

# Tenant ID for your Azure subscription
TENANT_ID = '<Your tenant ID>'

# Your service principal App ID
CLIENT = '<Your service principal ID>'

# Your service principal password
KEY = '<Your service principal password>'

credentials = ServicePrincipalCredentials(
    client_id = CLIENT,
    secret = KEY,
    tenant = TENANT_ID
)
```

> [NOTE!] 若要连接到 Azure 主权云之一，请使用 `cloud_environment` 参数。
>
> ```python
> from azure.common.credentials import ServicePrincipalCredentials
> from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
> 
> # Tenant ID for your Azure Subscription
> TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
> 
> # Your Service Principal App ID
> CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'
> 
> # Your Service Principal Password
> KEY = 'password'
> 
> credentials = ServicePrincipalCredentials(
>     client_id = CLIENT,
>     secret = KEY,
>     tenant = TENANT_ID,
>     cloud_environment = AZURE_CHINA_CLOUD
> )
> ```

如需更高的控制度，我们建议使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包装器。 请参阅 ADAL 网站获取所有可用方案的列表和示例。 服务主体身份验证的示例：

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

可结合 `AdalAuthentication` 类使用所有 ADAL 有效调用。

接下来，创建客户端对象来开始使用 API：

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> [NOTE!] 如果使用 Azure 主权云，还必须在创建管理客户端时指定相应的基 URL（通过 `msrestazure.azure_cloud` 中的常量）。 例如，对于 Azure 中国云：
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a>基于文件的身份验证

最简单的身份验证方法是创建包含 Azure 服务主体凭据的 JSON 文件。 可使用以下 CLI 命令同时创建新的服务主体和此文件：

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

将此文件保存在系统上可供代码读取的安全位置。 在 shell 中将包含完整路径的环境变量设置为此文件：

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

若要自行创建该文件，请采用以下格式：

```json
{
    "clientId": "<Service principal ID>",
    "clientSecret": "<Service principal secret/password>",
    "subscriptionId": "<Subscription associated with the service principal>",
    "tenantId": "<The service principal's tenant>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

然后，可以使用客户端工厂创建任何客户端：

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a>使用 Azure 托管标识进行身份验证
Azure 中的资源可以通过 Azure 托管标识这种简单的方式来使用 SDK/CLI，无需创建特定凭据。

> [!IMPORTANT]
>
> 若要使用托管标识，必须从 Azure 资源（例如某个 Azure 函数或在 Azure 中运行的 VM）连接到 Azure。 若要了解如何为资源配置托管标识，请参阅[配置 Azure 资源的托管标识](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)和[如何使用 Azure 资源的托管标识](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)。

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

## <a name="mgmt-auth-cli"></a>基于 CLI 的身份验证

SDK 能够使用 Azure CLI 的活动订阅创建客户端。

> [!IMPORTANT]
> 应将此方法用作开发人员快速入门体验。 对于生产用途，请使用 [ADAL](#mgmt-auth-legacy) 或自己的凭据系统。
> 对 CLI 配置进行任何更改会影响 SDK 的执行。

若要定义活动的凭据，请使用 [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。
默认的订阅 ID 是你拥有的唯一 ID，或使用 [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) 定义的 ID

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a>使用令牌凭据进行身份验证（传统方法）

以前的 SDK 版本尚未推出 ADAL，而是提供一个 `UserPassCredentials` 类。 此类被视为已弃用，不应继续使用。

此示例演示用户/密码方案。 此方案不支持 2FA。

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```
