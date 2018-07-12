---
title: 用于 Python 的 Azure Key Vault 库
description: 用于 Azure Key Vault 的 Python 客户端库的参考文档
author: lisawong19
keywords: Azure, Python, SDK, API, 密钥, Key Vault, 身份验证, 机密, 密钥, 安全
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 555f55dcf7355a1a82dc3ca5826e0d0bd3fad414
ms.sourcegitcommit: 8476146ae9bcd1533db47adbe2524b27b93aaba0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2018
ms.locfileid: "37925938"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="ecc96-104">用于 Python 的 Azure Key Vault 库</span><span class="sxs-lookup"><span data-stu-id="ecc96-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="ecc96-105">概述</span><span class="sxs-lookup"><span data-stu-id="ecc96-105">Overview</span></span>

<span data-ttu-id="ecc96-106">使用客户端库在 Azure Key Vault 中创建、更新和删除密钥与机密。</span><span class="sxs-lookup"><span data-stu-id="ecc96-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="ecc96-107">使用 Azure Key Vault 管理库来创建 Key Vault、授权应用程序及管理权限。</span><span class="sxs-lookup"><span data-stu-id="ecc96-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="ecc96-108">详细了解 [Azure Key Vault](/azure/key-vault/key-vault-whatis)。</span><span class="sxs-lookup"><span data-stu-id="ecc96-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="ecc96-109">安装库</span><span class="sxs-lookup"><span data-stu-id="ecc96-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="ecc96-110">客户端库</span><span class="sxs-lookup"><span data-stu-id="ecc96-110">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="ecc96-111">示例</span><span class="sxs-lookup"><span data-stu-id="ecc96-111">Examples</span></span>

<span data-ttu-id="ecc96-112">从 Key Vault 检索[JSON Web 密钥](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。</span><span class="sxs-lookup"><span data-stu-id="ecc96-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```

<span data-ttu-id="ecc96-113">同样，可以使用下面的代码片段从保管库检索机密：</span><span class="sxs-lookup"><span data-stu-id="ecc96-113">Similarly, you can use the following snippet to retrieve a secret from the vault:</span></span>

```
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '',
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

secret_bundle = client.get_secret("https://VAULT_ID.vault.azure.net/", "SECRET_ID", "SECRET_VERSION")

print(secret_bundle.value)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ecc96-114">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="ecc96-114">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a><span data-ttu-id="ecc96-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="ecc96-115">Management API</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="ecc96-116">示例</span><span class="sxs-lookup"><span data-stu-id="ecc96-116">Example</span></span>
<span data-ttu-id="ecc96-117">以下示例演示如何创建 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="ecc96-117">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="ecc96-118">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="ecc96-118">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

> [!div class="nextstepaction"]
> [<span data-ttu-id="ecc96-119">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="ecc96-119">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="ecc96-120">示例</span><span class="sxs-lookup"><span data-stu-id="ecc96-120">Samples</span></span>
* <span data-ttu-id="ecc96-121">[管理 Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="ecc96-121">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="ecc96-122">[Key Vault 恢复][2]</span><span class="sxs-lookup"><span data-stu-id="ecc96-122">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="ecc96-123">查看 Azure Key Vault 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。</span><span class="sxs-lookup"><span data-stu-id="ecc96-123">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 
