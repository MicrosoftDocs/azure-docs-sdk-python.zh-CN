---
title: 用于 Python 的 Azure Web 应用库
description: ''
keywords: Azure, Python, SDK, API, web 应用, 应用服务
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 26b578d9edc7023c06d4c9bfc8c8fb44a169c40d
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534177"
---
# <a name="azure-web-apps-libraries-for-python"></a>用于 Python 的 Azure Web 应用库

## <a name="overview"></a>概述

使用 [Azure 应用服务](/azure/app-service)部署和缩放网站、Web 应用程序、服务与 REST API。

若要开始使用 Azure 应用服务，请参阅[在 Azure 中创建 Python Web 应用](/azure/app-service-web/app-service-web-get-started-python)。

## <a name="management-api"></a>管理 API

使用管理 API 来部署、管理和缩放 Azure 应用服务中托管的元素。

通过 pip 安装库。

```bash
pip install azure-mgmt-web
```

### <a name="example"></a>示例

将 GitHub 存储库中的 Web 应用部署到 Azure Web 应用。

```python
siteConfiguration = SiteConfig(
    python_version='3.4'
)

# create a web app
web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=SERVICE_PLAN_ID,
        site_config=siteConfiguration
    )
)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url='https://github.com/lisawong19/python-docs-hello-world',
        branch='master'
    )
)
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/webapps/management)

## <a name="samples"></a>示例

* [使用 Python 管理 Azure 网站][1]
* [创建逻辑应用工作流][2]

查看 Web 应用程序示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app)。

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md
