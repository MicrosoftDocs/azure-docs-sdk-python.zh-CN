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
# <a name="azure-key-vault-libraries-for-python"></a>用于 Python 的 Azure Key Vault 库

[Azure Key Vault](/azure/key-vault/) 是 Azure 的存储和管理系统，用于管理加密密钥、机密和证书。 用于 Key Vault 的 Python SDK API 在客户端库和管理库之间进行了拆分。

使用客户端库可以执行以下操作：
- 访问、更新或删除存储在 Azure Key Vault 中的项目
- 获取存储的证书的元数据
- 针对 Key Vault 中的对称密钥验证签名

使用管理库可以执行以下操作：
- 创建、更新或删除新的 Key Vault 存储
- 控制保管库访问策略
- 按订阅或资源组列出保管库
- 检查保管库名称可用性

## <a name="install-the-libraries"></a>安装库

### <a name="client-library"></a>客户端库

```bash
pip install azure-keyvault
```

## <a name="examples"></a>示例

以下示例使用服务主体身份验证，对于连接到 Azure 的应用程序而言，该身份验证是建议的登录方法。 若要了解服务主体身份验证，请参阅[使用用于 Python 的 Azure SDK 进行身份验证](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)

从保管库中检索非对称密钥的公共部分：

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

从保管库中检索机密：

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
> [了解客户端 API](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a>管理库

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>示例

以下示例演示如何创建 Azure Key Vault。 

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
> [了解管理 API](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>示例
* [管理 Azure Key Vault][1] 
* [Azure Key Vault 恢复][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

查看 Azure Key Vault 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。 
