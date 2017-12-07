---
title: "用于 Python 的 Azure 库"
description: "用于 Python 的 Azure 管理和服务库概述"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 7c069f849007ea2c02cf4347ce213dd033dcd68b
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2017
---
# <a name="azure-libraries-for-python"></a><span data-ttu-id="43051-104">用于 Python 的 Azure 库</span><span class="sxs-lookup"><span data-stu-id="43051-104">Azure libraries for Python</span></span>

<span data-ttu-id="43051-105">借助用于 Python 的 Azure 库可以通过应用程序代码使用 Azure 服务和管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="43051-105">The Azure libraries for Python let you use Azure services and manage Azure resources from your application code.</span></span> <span data-ttu-id="43051-106">这些库在 [PyPI](python-sdk-azure-install.md) 在中提供，方便在 Python 项目中使用。</span><span class="sxs-lookup"><span data-stu-id="43051-106">The libraries are available in [PyPI](python-sdk-azure-install.md) for use in your Python projects.</span></span>

## <a name="manage-azure-resources"></a><span data-ttu-id="43051-107">管理 Azure 资源</span><span class="sxs-lookup"><span data-stu-id="43051-107">Manage Azure resources</span></span>

<span data-ttu-id="43051-108">使用用于 Python 的 Azure 库通过 Python 应用程序创建和管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="43051-108">Create and manage Azure resources from Python applications using the Azure libraries for Python.</span></span>

<span data-ttu-id="43051-109">例如，若要创建 SQL Server 实例，可使用以下代码：</span><span class="sxs-lookup"><span data-stu-id="43051-109">For example, to create a SQL Server instance, you can use the following code:</span></span>

```python
sql_client = SqlManagementClient(
    credentials,
    subscription_id
)

server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

<span data-ttu-id="43051-110">查看[安装说明](python-sdk-azure-install.md)了解库的完整列表以及如何将其导入项目，然后阅读[入门文章](python-sdk-azure-get-started.yml)来设置身份验证并针对自己的 Azure 订阅运行示例代码。</span><span class="sxs-lookup"><span data-stu-id="43051-110">Review the [install instructions](python-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](python-sdk-azure-get-started.yml) to set up your authentication and run sample code against your own Azure subscription.</span></span>

## <a name="connect-to-azure-services"></a><span data-ttu-id="43051-111">连接到 Azure 服务</span><span class="sxs-lookup"><span data-stu-id="43051-111">Connect to Azure services</span></span>

<span data-ttu-id="43051-112">使用 Python 库除了可以在 Azure 中创建和管理资源以外，还可以连接并使用应用中的资源。</span><span class="sxs-lookup"><span data-stu-id="43051-112">In addition to using Python libraries to create and manage resources within Azure, you can also use Python libraries to connect and use those resources in your apps.</span></span> <span data-ttu-id="43051-113">例如，可以更新表 SQL 数据库或者在 Azure 存储中存储文件。</span><span class="sxs-lookup"><span data-stu-id="43051-113">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="43051-114">从库的完整列表中选择特定服务所需的库，并访问 Python 开发人员中心获取教程和示例代码来帮助自己在应用中使用这些库。</span><span class="sxs-lookup"><span data-stu-id="43051-114">Select the library you need for a particular service from the complete list of libraries and visit the Python developer center for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="43051-115">例如，若要在 Blob 中上传简单的 HTML 页面并获取 URL，请使用以下代码：</span><span class="sxs-lookup"><span data-stu-id="43051-115">For example, to upload a simple HTML page on a blob and get the Url:</span></span>

```python
storage_client = CloudStorageAccount(storage_account_name, storage_key)
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(
    'mycontainername',
    public_access=PublicAccess.Blob
)

blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url('mycontainername', 'myblobname'))
```

## <a name="sample-code-and-reference"></a><span data-ttu-id="43051-116">代码示例和参考</span><span class="sxs-lookup"><span data-stu-id="43051-116">Sample code and reference</span></span>
<span data-ttu-id="43051-117">以下示例涵盖可以通过用于 Python 的 Azure 管理库完成的常见自动化任务，并提供可在自己应用中使用的现成代码：</span><span class="sxs-lookup"><span data-stu-id="43051-117">The following samples cover common automation tasks with the Azure management libraries for Python and have code ready to use in your own apps:</span></span>
- [<span data-ttu-id="43051-118">虚拟机</span><span class="sxs-lookup"><span data-stu-id="43051-118">Virtual Machines</span></span>](python-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="43051-119">Web 应用</span><span class="sxs-lookup"><span data-stu-id="43051-119">Web apps</span></span>](python-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="43051-120">SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="43051-120">SQL Database</span></span>](python-sdk-azure-sql-database-samples.md)

<span data-ttu-id="43051-121">我们针对服务和管理库中的所有包提供了[参考](/python/api/overview/azure)文档。</span><span class="sxs-lookup"><span data-stu-id="43051-121">A [reference](/python/api/overview/azure) is available for all packages in both the service an management libraries.</span></span> <span data-ttu-id="43051-122">[发行说明](python-sdk-azure-release-notes.md)中介绍了新增功能、重大更改，并提供了有关如何从以前的版本迁移的说明。</span><span class="sxs-lookup"><span data-stu-id="43051-122">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](python-sdk-azure-release-notes.md).</span></span> 

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="43051-123">获取帮助和提供反馈</span><span class="sxs-lookup"><span data-stu-id="43051-123">Get help and give feedback</span></span>

<span data-ttu-id="43051-124">请在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) 社区中提问，在[项目 GitHub](https://github.com/Azure/azure-sdk-for-python) 中反映有关 SDK 的问题。</span><span class="sxs-lookup"><span data-stu-id="43051-124">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) and open issues against the SDK on the [project GitHub](https://github.com/Azure/azure-sdk-for-python).</span></span>
