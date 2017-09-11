---
title: "用于 Python 的 Azure Key Vault 库"
description: "用于 Azure Key Vault 的 Python 客户端库的参考文档"
author: lisawong19
keywords: "Azure, Python, SDK, API, 密钥, Key Vault, 身份验证, 机密, 密钥, 安全"
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 3eac46eb4d5d19273ead9f19b739f6fb6d72e5cc
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="abe6c-104">用于 Python 的 Azure Key Vault 库</span><span class="sxs-lookup"><span data-stu-id="abe6c-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="abe6c-105">概述</span><span class="sxs-lookup"><span data-stu-id="abe6c-105">Overview</span></span>

<span data-ttu-id="abe6c-106">使用客户端库在 Azure Key Vault 中创建、更新和删除密钥与机密。</span><span class="sxs-lookup"><span data-stu-id="abe6c-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="abe6c-107">使用 Azure Key Vault 管理库来创建 Key Vault、授权应用程序及管理权限。</span><span class="sxs-lookup"><span data-stu-id="abe6c-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="abe6c-108">详细了解 [Azure Key Vault](/azure/key-vault/key-vault-whatis)。</span><span class="sxs-lookup"><span data-stu-id="abe6c-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="abe6c-109">安装库</span><span class="sxs-lookup"><span data-stu-id="abe6c-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="abe6c-110">客户端库</span><span class="sxs-lookup"><span data-stu-id="abe6c-110">Client library</span></span>
```bash
pip install azure-keyvault
```

## <a name="example"></a><span data-ttu-id="abe6c-111">示例</span><span class="sxs-lookup"><span data-stu-id="abe6c-111">Example</span></span>
<span data-ttu-id="abe6c-112">从 Key Vault 检索[JSON Web 密钥](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。</span><span class="sxs-lookup"><span data-stu-id="abe6c-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callack(server, resource, scope):
    credentials = credentials or ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = resource
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callack))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```
[!div class="nextstepaction"]
[<span data-ttu-id="abe6c-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="abe6c-113">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/clientlibrary)

### <a name="management-api"></a><span data-ttu-id="abe6c-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="abe6c-114">Management API</span></span>
```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="abe6c-115">示例</span><span class="sxs-lookup"><span data-stu-id="abe6c-115">Example</span></span>
<span data-ttu-id="abe6c-116">以下示例演示如何创建 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="abe6c-116">The following example shows how to create an Azure Key Vault.</span></span> 

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
> [<span data-ttu-id="abe6c-117">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="abe6c-117">Explore the Management APIs</span></span>](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [<span data-ttu-id="abe6c-118">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="abe6c-118">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/managementlibrary)

## <a name="samples"></a><span data-ttu-id="abe6c-119">示例</span><span class="sxs-lookup"><span data-stu-id="abe6c-119">Samples</span></span>
* <span data-ttu-id="abe6c-120">[管理 Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="abe6c-120">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="abe6c-121">[Key Vault 恢复][2]</span><span class="sxs-lookup"><span data-stu-id="abe6c-121">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="abe6c-122">查看 Azure Key Vault 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。</span><span class="sxs-lookup"><span data-stu-id="abe6c-122">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 