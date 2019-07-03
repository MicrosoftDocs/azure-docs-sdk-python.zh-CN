---
title: 用于 Python 的 Azure Key Vault 库
description: 用于 Azure Key Vault 的 Python 客户端库的参考文档
author: sptramer
manager: carmonm
ms.author: sttramer
ms.date: 06/10/2019
ms.topic: conceptual
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: f4661ee389c13ce8546e7b5cc8866ab7b216d3b0
ms.sourcegitcommit: 92fa5dbcfd9a20f4a49da5f4bdc03045783d3495
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "67149328"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="06dc6-103">用于 Python 的 Azure Key Vault 库</span><span class="sxs-lookup"><span data-stu-id="06dc6-103">Azure Key Vault libraries for Python</span></span>

<span data-ttu-id="06dc6-104">[Azure Key Vault](/azure/key-vault/) 是 Azure 的存储和管理系统，用于管理加密密钥、机密和证书。</span><span class="sxs-lookup"><span data-stu-id="06dc6-104">[Azure Key Vault](/azure/key-vault/) is Azure's storage and management system for cryptographic keys, secrets, and certificate management.</span></span> <span data-ttu-id="06dc6-105">用于 Key Vault 的 Python SDK API 在客户端库和管理库之间进行了拆分。</span><span class="sxs-lookup"><span data-stu-id="06dc6-105">The Python SDK API for Key Vault is split between client libraries and management libraries.</span></span>

<span data-ttu-id="06dc6-106">使用客户端库可以执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="06dc6-106">Use the client library to:</span></span>
- <span data-ttu-id="06dc6-107">访问、更新或删除存储在 Azure Key Vault 中的项目</span><span class="sxs-lookup"><span data-stu-id="06dc6-107">Access, update, or delete items stored in an Azure Key Vault</span></span>
- <span data-ttu-id="06dc6-108">获取存储的证书的元数据</span><span class="sxs-lookup"><span data-stu-id="06dc6-108">Get metadata for stored certificates</span></span>
- <span data-ttu-id="06dc6-109">针对 Key Vault 中的对称密钥验证签名</span><span class="sxs-lookup"><span data-stu-id="06dc6-109">Verify signatures against symmetric keys in Key Vault</span></span>

<span data-ttu-id="06dc6-110">使用管理库可以执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="06dc6-110">Use the management library to:</span></span>
- <span data-ttu-id="06dc6-111">创建、更新或删除新的 Key Vault 存储</span><span class="sxs-lookup"><span data-stu-id="06dc6-111">Create, update, or delete new Key Vault stores</span></span>
- <span data-ttu-id="06dc6-112">控制保管库访问策略</span><span class="sxs-lookup"><span data-stu-id="06dc6-112">Control vault access policies</span></span>
- <span data-ttu-id="06dc6-113">按订阅或资源组列出保管库</span><span class="sxs-lookup"><span data-stu-id="06dc6-113">List vaults by subscription or resource group</span></span>
- <span data-ttu-id="06dc6-114">检查保管库名称可用性</span><span class="sxs-lookup"><span data-stu-id="06dc6-114">Check for vault name availability</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="06dc6-115">安装库</span><span class="sxs-lookup"><span data-stu-id="06dc6-115">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="06dc6-116">客户端库</span><span class="sxs-lookup"><span data-stu-id="06dc6-116">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="06dc6-117">示例</span><span class="sxs-lookup"><span data-stu-id="06dc6-117">Examples</span></span>

<span data-ttu-id="06dc6-118">以下示例使用服务主体身份验证，对于连接到 Azure 的应用程序而言，该身份验证是建议的登录方法。</span><span class="sxs-lookup"><span data-stu-id="06dc6-118">The following examples use service principal authentication, which is the recommended sign in method for applications that connect to Azure.</span></span> <span data-ttu-id="06dc6-119">若要了解服务主体身份验证，请参阅[使用用于 Python 的 Azure SDK 进行身份验证](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span><span class="sxs-lookup"><span data-stu-id="06dc6-119">To learn about service principal authentication, see [Authenticate with the Azure SDK for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span></span>

<span data-ttu-id="06dc6-120">从保管库中检索非对称密钥的公共部分：</span><span class="sxs-lookup"><span data-stu-id="06dc6-120">Retrieve the public portion of an asymmetric key from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# KEY_VERSION is required, and can be obtained with the KeyVaultClient.get_key_versions(self, vault_url, key_name) API
key_bundle = client.get_key(VAULT_URL, KEY_NAME, KEY_VERSION)
key = key_bundle.key
```

<span data-ttu-id="06dc6-121">从保管库中检索机密：</span><span class="sxs-lookup"><span data-stu-id="06dc6-121">Retrieve a secret from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# SECRET_VERSION is required, and can be obtained with the KeyVaultClient.get_secret_versions(self, vault_url, secret_id) API
secret_bundle = client.get_secret(VAULT_URL, SECRET_ID, SECRET_VERSION)
secret = secret_bundle.value
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="06dc6-122">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="06dc6-122">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a><span data-ttu-id="06dc6-123">管理库</span><span class="sxs-lookup"><span data-stu-id="06dc6-123">Management library</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="06dc6-124">示例</span><span class="sxs-lookup"><span data-stu-id="06dc6-124">Example</span></span>

<span data-ttu-id="06dc6-125">以下示例演示如何创建 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="06dc6-125">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient
from azure.common.credentials import ServicePrincipalCredentials


credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

# Even when using service principal credentials, a subscription ID is required. For service principals,
# this should be the subscription used to create the service principal. Storing a token like a valid
# subscription ID in code is not recommended and only shown here for example purposes.
SUBSCRIPTION_ID = '...'
client = KeyVaultManagementClient(credentials, SUBSCRIPTION_ID)

# The object ID and organization ID (tenant) of the user, application, or service principal for access policies.
# These values can be found through the Azure CLI or the Portal.
ALLOW_OBJECT_ID = '...'
ALLOW_TENANT_ID = '...'

RESOURCE_GROUP = '...'
VAULT_NAME = '...'

# Vault properties may also be created by using the azure.mgmt.keyvault.models.VaultCreateOrUpdateParameters
# class, rather than a map. 
operation = client.vaults.create_or_update(
    RESOURCE_GROUP,
    VAULT_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': TENANT_ID,
            'access_policies': [{
                'object_id': OBJECT_ID,
                'tenant_id': ALLOW_TENANT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)

vault = operation.result()
print(f'New vault URI: {vault.properties.vault_uri}')
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="06dc6-126">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="06dc6-126">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="06dc6-127">示例</span><span class="sxs-lookup"><span data-stu-id="06dc6-127">Samples</span></span>
* <span data-ttu-id="06dc6-128">[管理 Azure Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="06dc6-128">[Manage Azure Key Vaults][1]</span></span> 
* <span data-ttu-id="06dc6-129">[Azure Key Vault 恢复][2]</span><span class="sxs-lookup"><span data-stu-id="06dc6-129">[Azure Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="06dc6-130">查看 Azure Key Vault 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。</span><span class="sxs-lookup"><span data-stu-id="06dc6-130">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 
