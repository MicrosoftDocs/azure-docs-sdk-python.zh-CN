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
# <a name="azure-libraries-for-python"></a>用于 Python 的 Azure 库

借助用于 Python 的 Azure 库可以通过应用程序代码使用 Azure 服务和管理 Azure 资源。 这些库在 [PyPI](python-sdk-azure-install.md) 在中提供，方便在 Python 项目中使用。

## <a name="manage-azure-resources"></a>管理 Azure 资源

使用用于 Python 的 Azure 库通过 Python 应用程序创建和管理 Azure 资源。

例如，若要创建 SQL Server 实例，可使用以下代码：

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

查看[安装说明](python-sdk-azure-install.md)了解库的完整列表以及如何将其导入项目，然后阅读[入门文章](python-sdk-azure-get-started.yml)来设置身份验证并针对自己的 Azure 订阅运行示例代码。

## <a name="connect-to-azure-services"></a>连接到 Azure 服务

使用 Python 库除了可以在 Azure 中创建和管理资源以外，还可以连接并使用应用中的资源。 例如，可以更新表 SQL 数据库或者在 Azure 存储中存储文件。 从库的完整列表中选择特定服务所需的库，并访问 Python 开发人员中心获取教程和示例代码来帮助自己在应用中使用这些库。

例如，若要在 Blob 中上传简单的 HTML 页面并获取 URL，请使用以下代码：

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

## <a name="sample-code-and-reference"></a>代码示例和参考
以下示例涵盖可以通过用于 Python 的 Azure 管理库完成的常见自动化任务，并提供可在自己应用中使用的现成代码：
- [虚拟机](python-sdk-azure-virtual-machine-samples.md)
- [Web 应用](python-sdk-azure-web-apps-samples.md)
- [SQL 数据库](python-sdk-azure-sql-database-samples.md)

我们针对服务和管理库中的所有包提供了[参考](/python/api/overview/azure)文档。 [发行说明](python-sdk-azure-release-notes.md)中介绍了新增功能、重大更改，并提供了有关如何从以前的版本迁移的说明。 

## <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

请在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) 社区中提问，在[项目 GitHub](https://github.com/Azure/azure-sdk-for-python) 中反映有关 SDK 的问题。
