---
title: 多云
description: 在所有区域上使用 Azure
author: lmazuel
ms.author: lmazuel
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: a4d006e6244bf6fb1151e32583e8bc3a642d4663
ms.sourcegitcommit: 8a9e4295359a4f47b21908541e2460c333e94a0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2018
ms.locfileid: "39624973"
---
# <a name="multi-cloud---use-azure-on-all-regions"></a><span data-ttu-id="30f86-103">多云 - 在所有区域上使用 Azure</span><span class="sxs-lookup"><span data-stu-id="30f86-103">Multi-cloud - use Azure on all regions</span></span>

<span data-ttu-id="30f86-104">可以使用用于 Python 的 Azure SDK 连接到[提供](https://azure.microsoft.com/regions/services) Azure 的所有区域。</span><span class="sxs-lookup"><span data-stu-id="30f86-104">You can use the Azure SDK for Python to connect to all regions where Azure is [available](https://azure.microsoft.com/regions/services).</span></span>

<span data-ttu-id="30f86-105">默认情况下，用于 Python 的 Azure SDK 配置为连接到公共 Azure。</span><span class="sxs-lookup"><span data-stu-id="30f86-105">By default, the Azure SDK for Python is configured to connect to public Azure.</span></span>

## <a name="using-predeclared-cloud-definition"></a><span data-ttu-id="30f86-106">使用预先声明的云定义</span><span class="sxs-lookup"><span data-stu-id="30f86-106">Using predeclared cloud definition</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30f86-107">对于本部分而言，`msrestazure` 包版本必须高于或等于 0.4.11。</span><span class="sxs-lookup"><span data-stu-id="30f86-107">The `msrestazure` package must be superior or equals to 0.4.11 for this section.</span></span>

<span data-ttu-id="30f86-108">可以使用 `msrestazure` 的 `azure_cloud` 模块</span><span class="sxs-lookup"><span data-stu-id="30f86-108">You can use the `azure_cloud` module of `msrestazure`</span></span>

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=AZURE_CHINA_CLOUD
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager
)
``` 
  
<span data-ttu-id="30f86-109">可用的云定义为</span><span class="sxs-lookup"><span data-stu-id="30f86-109">Available cloud definition are</span></span>
  - <span data-ttu-id="30f86-110">AZURE_PUBLIC_CLOUD</span><span class="sxs-lookup"><span data-stu-id="30f86-110">AZURE_PUBLIC_CLOUD</span></span>
  - <span data-ttu-id="30f86-111">AZURE_CHINA_CLOUD</span><span class="sxs-lookup"><span data-stu-id="30f86-111">AZURE_CHINA_CLOUD</span></span>
  - <span data-ttu-id="30f86-112">AZURE_US_GOV_CLOUD</span><span class="sxs-lookup"><span data-stu-id="30f86-112">AZURE_US_GOV_CLOUD</span></span>
  - <span data-ttu-id="30f86-113">AZURE_GERMAN_CLOUD</span><span class="sxs-lookup"><span data-stu-id="30f86-113">AZURE_GERMAN_CLOUD</span></span>

## <a name="using-your-own-cloud-definition-eg-azure-stack"></a><span data-ttu-id="30f86-114">使用自己的云定义（例如 Azure Stack）</span><span class="sxs-lookup"><span data-stu-id="30f86-114">Using your own cloud definition (e.g. Azure Stack)</span></span>
<span data-ttu-id="30f86-115">ARM 包含可为你提供帮助的元数据终结点：</span><span class="sxs-lookup"><span data-stu-id="30f86-115">ARM has a metadata endpoint to help you:</span></span>

```python
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

mystack_cloud = get_cloud_from_metadata_endpoint("https://myazurestack-arm-endpoint.com")
credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=mystack_cloud
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=mystack_cloud.endpoints.resource_manager
)
```
## <a name="using-adal"></a><span data-ttu-id="30f86-116">使用 ADAL</span><span class="sxs-lookup"><span data-stu-id="30f86-116">Using ADAL</span></span>

<span data-ttu-id="30f86-117">若要连接到另一个区域，必须考虑以下事项：</span><span class="sxs-lookup"><span data-stu-id="30f86-117">To connect to another region, a few things have to be considered:</span></span>

- <span data-ttu-id="30f86-118">用于请求令牌（身份验证）的终结点是什么？</span><span class="sxs-lookup"><span data-stu-id="30f86-118">What is the endpoint where to ask for a token (authentication)?</span></span>
- <span data-ttu-id="30f86-119">我要在其中使用此令牌（使用情况）的终结点是什么？</span><span class="sxs-lookup"><span data-stu-id="30f86-119">What is the endpoint where I will use this token (usage)?</span></span>

<span data-ttu-id="30f86-120">这是一个一般示例：</span><span class="sxs-lookup"><span data-stu-id="30f86-120">This is a generic example:</span></span>

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Public Azure - default values
authentication_endpoint = 'https://login.microsoftonline.com/'
azure_endpoint = 'https://management.azure.com/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-government"></a><span data-ttu-id="30f86-121">Azure Government </span><span class="sxs-lookup"><span data-stu-id="30f86-121">Azure Government</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Government
authentication_endpoint = 'https://login.microsoftonline.us/'
azure_endpoint = 'https://management.usgovcloudapi.net/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-germany"></a><span data-ttu-id="30f86-122">Azure 德国</span><span class="sxs-lookup"><span data-stu-id="30f86-122">Azure Germany</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure Germany
authentication_endpoint = 'https://login.microsoftonline.de/'
azure_endpoint = 'https://management.microsoftazure.de/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-china"></a><span data-ttu-id="30f86-123">Azure 中国</span><span class="sxs-lookup"><span data-stu-id="30f86-123">Azure China</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure China
authentication_endpoint = 'https://login.chinacloudapi.cn/'
azure_endpoint = 'https://management.chinacloudapi.cn/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```
