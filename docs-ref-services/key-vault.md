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
ms.openlocfilehash: 6f0f1012839dad21fb8140dbbdf0f883d2877317
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
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

## <a name="example"></a>示例
从 Key Vault 检索[JSON Web 密钥](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。

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
[了解客户端 API](/python/api/overview/azure/keyvault/client)

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
> [了解管理 API](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>示例
* [管理 Key Vault][1] 
* [Key Vault 恢复][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

查看 Azure Key Vault 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。 