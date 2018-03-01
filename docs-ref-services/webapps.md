---
title: "用于 Python 的 Azure Web 应用库"
description: 
keywords: "Azure, Python, SDK, API, web 应用, 应用服务"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 8e8dd78cbc2d5887308361a47a9571ce242aee6e
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="azure-web-apps-libraries-for-python"></a><span data-ttu-id="23886-103">用于 Python 的 Azure Web 应用库</span><span class="sxs-lookup"><span data-stu-id="23886-103">Azure Web Apps libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="23886-104">概述</span><span class="sxs-lookup"><span data-stu-id="23886-104">Overview</span></span>

<span data-ttu-id="23886-105">使用 [Azure 应用服务](/azure/app-service)部署和缩放网站、Web 应用程序、服务与 REST API。</span><span class="sxs-lookup"><span data-stu-id="23886-105">Deploy and scale websites, web applications, services, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="23886-106">若要开始使用 Azure 应用服务，请参阅[在 Azure 中创建 Python Web 应用](/azure/app-service-web/app-service-web-get-started-python)。</span><span class="sxs-lookup"><span data-stu-id="23886-106">To get started with Azure App Service, see [Create a Python web app in Azure](/azure/app-service-web/app-service-web-get-started-python).</span></span>

## <a name="management-api"></a><span data-ttu-id="23886-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="23886-107">Management API</span></span>

<span data-ttu-id="23886-108">使用管理 API 来部署、管理和缩放 Azure 应用服务中托管的元素。</span><span class="sxs-lookup"><span data-stu-id="23886-108">Deploy, manage, and scale elements hosted in the Azure App Service with the management API.</span></span>

<span data-ttu-id="23886-109">通过 pip 安装库。</span><span class="sxs-lookup"><span data-stu-id="23886-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-web
```

### <a name="example"></a><span data-ttu-id="23886-110">示例</span><span class="sxs-lookup"><span data-stu-id="23886-110">Example</span></span>

<span data-ttu-id="23886-111">将 GitHub 存储库中的 Web 应用部署到 Azure Web 应用。</span><span class="sxs-lookup"><span data-stu-id="23886-111">Deploy a webapp from a GitHub repository into Azure Web App.</span></span>

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
> [<span data-ttu-id="23886-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="23886-112">Explore the Management APIs</span></span>](/python/api/overview/azure/webapps/management)

## <a name="samples"></a><span data-ttu-id="23886-113">示例</span><span class="sxs-lookup"><span data-stu-id="23886-113">Samples</span></span> 

* <span data-ttu-id="23886-114">[使用 Python 管理 Azure 网站][1]</span><span class="sxs-lookup"><span data-stu-id="23886-114">[Manage Azure websites with python][1]</span></span>
* <span data-ttu-id="23886-115">[创建逻辑应用工作流][2]</span><span class="sxs-lookup"><span data-stu-id="23886-115">[Create a Logic App workflow][2]</span></span>
 
<span data-ttu-id="23886-116">查看 Web 应用程序示例的[完整列表](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=web-app)。</span><span class="sxs-lookup"><span data-stu-id="23886-116">View the [complete list](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=web-app) of web application samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md