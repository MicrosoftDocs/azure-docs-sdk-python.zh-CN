---
title: "用于 Python 的 Azure Active Directory 库"
description: "Azure Active Directory Python 客户端库的参考文档"
keywords: "Azure, Python, SDK, API, SQL, 身份验证, AAD, Active Directory , Graph, OAuth 2.0"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/07/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: active-directory
ms.openlocfilehash: 41234fd44fa98c1ff57287193b0437b7caca46c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-libraries-for-python"></a><span data-ttu-id="29992-104">用于 Python 的 Azure Active Directory 库</span><span class="sxs-lookup"><span data-stu-id="29992-104">Azure Active Directory libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="29992-105">概述</span><span class="sxs-lookup"><span data-stu-id="29992-105">Overview</span></span>

<span data-ttu-id="29992-106">使用 [Azure Active Directory](/azure/active-directory/active-directory-whatis) 将用户登录并控制对应用程序和 API 的访问。</span><span class="sxs-lookup"><span data-stu-id="29992-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

## <a name="client-library"></a><span data-ttu-id="29992-107">客户端库</span><span class="sxs-lookup"><span data-stu-id="29992-107">Client library</span></span>

<span data-ttu-id="29992-108">使用[用于 Python 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 配置 OAuth2、OpenID Connect 或 Active Directory Graph 身份验证和 [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) 单一登录。</span><span class="sxs-lookup"><span data-stu-id="29992-108">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Python](https://github.com/AzureAD/azure-activedirectory-library-for-python).</span></span>

```bash
pip install azure-graphrbac
```

### <a name="example"></a><span data-ttu-id="29992-109">示例</span><span class="sxs-lookup"><span data-stu-id="29992-109">Example</span></span>
> [!NOTE]
> <span data-ttu-id="29992-110">创建凭据实例时，需将资源参数更改为 https://graph.windows.net</span><span class="sxs-lookup"><span data-stu-id="29992-110">You need to change the resource parameter to https://graph.windows.net while creating the credentials instance</span></span>

```python
from azure.graphrbac import GraphRbacManagementClient
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
            'user@domain.com',      # Your user
            'my_password',          # Your password
            resource="https://graph.windows.net"
    )

tenant_id = "myad.onmicrosoft.com"

graphrbac_client = GraphRbacManagementClient(
    credentials,
    tenant_id
)
```
<span data-ttu-id="29992-111">以下代码创建一个用户，直接获取再通过列表筛选获取该用户，然后将其删除。</span><span class="sxs-lookup"><span data-stu-id="29992-111">The following code creates a user, get it directly and by list filtering, and then delete it.</span></span>
```python
from azure.graphrbac.models import UserCreateParameters, PasswordProfile

user = graphrbac_client.users.create(
    UserCreateParameters(
        user_principal_name="testbuddy@{}".format(MY_AD_DOMAIN),
        account_enabled=False,
        display_name='Test Buddy',
        mail_nickname='testbuddy',
        password_profile=PasswordProfile(
            password='MyStr0ngP4ssword',
            force_change_password_next_login=True
        )
    )
)
# user is a User instance
self.assertEqual(user.display_name, 'Test Buddy')

user = graphrbac_client.users.get(user.object_id)
self.assertEqual(user.display_name, 'Test Buddy')

for user in graphrbac_client.users.list(filter="displayName eq 'Test Buddy'"):
    self.assertEqual(user.display_name, 'Test Buddy')

graphrbac_client.users.delete(user.object_id)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="29992-112">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="29992-112">Explore the Client APIs</span></span>](/python/api/overview/azure/activedirectory/clientlibrary?)

<span data-ttu-id="29992-113">详细了解可在应用中使用的 [Azure AD 示例 Python 代码](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python)。</span><span class="sxs-lookup"><span data-stu-id="29992-113">Explore more [sample Python code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python) you can use in your apps.</span></span>