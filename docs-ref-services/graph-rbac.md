---
title: 用于 Python 的 Graph RBAC 库
description: 用于 Python 的 Graph RBAC 库参考
keywords: Azure, python, SDK, API, Graph RBAC
author: rloutlaw
ms.author: routlaw
manager: jfriend
ms.date: 05/10/2019
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: e9b0aba7998565284ae18e0036da96d033b2b05f
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534278"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a>用于 Python 的 Azure Active Directory Graph 库

> [!IMPORTANT]
>
> 从 2019 年 2 月开始，我们开始弃用某些早期版本的 Azure Active Directory Graph API，转而支持 Microsoft Graph API。 
>
> 有关详细信息、更新和期限，请参阅 Office 开发人员中心中的 [Microsoft Graph 或 Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph)。
>
> 展望未来，应用程序将使用 Microsoft Graph API。 

## <a name="overview"></a>概述 

使用 [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-api) 将用户登录并控制对应用程序和 API 的访问。    

## <a name="client-library"></a>客户端库   

 ```bash    
pip install azure-graphrbac 
``` 

### <a name="example"></a>示例 
> [!NOTE]   
> 创建凭据实例时，需将资源参数更改为 https://graph.windows.net    
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
以下代码创建一个用户，直接获取再通过列表筛选获取该用户，然后将其删除。   
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