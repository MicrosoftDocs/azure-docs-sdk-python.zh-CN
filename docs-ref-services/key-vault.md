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
# <a name="azure-key-vault-libraries-for-python"></a>用于 Python 的 Azure Key Vault 库

## <a name="overview"></a>概述

使用客户端库在 Azure Key Vault 中创建、更新和删除密钥与机密。

使用 Azure Key Vault 管理库来创建 Key Vault、授权应用程序及管理权限。 

详细了解 [Azure Key Vault](/azure/key-vault/key-vault-whatis)。

## <a name="install-the-libraries"></a>安装库

### <a name="client-library"></a>客户端库

```bash
pip install azure-keyvault
```

## <a name="examples"></a>示例

从 Key Vault 检索[JSON Web 密钥](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。

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

同样，可以使用下面的代码片段从保管库检索机密：

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
> [了解客户端 API](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a>管理 API

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>示例
以下示例演示如何创建 Azure Key Vault。 

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
> [了解客户端 API](/python/api/overview/azure/keyvault/client)

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>示例
* [管理 Key Vault][1] 
* [Key Vault 恢复][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

查看 Azure Key Vault 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。 
