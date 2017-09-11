---
title: "用于 Python 的 Azure SQL 数据库库"
description: 
keywords: "Azure, Python, SDK, API, SQL, 数据库, pyodbc"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: b580c5011412bc77fd8fd55b709a305be07e2316
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-sql-database-libraries-for-python"></a>用于 Python 的 Azure SQL 数据库库

## <a name="overview"></a>概述

使用 Microsoft ODBC 驱动程序和 pyodbc 通过 Python 处理 [Azure SQL 数据库](/azure/sql-database/sql-database-technical-overview)中存储的数据。 

## <a name="client-odbc-driver-and-pyodbc"></a>客户端 ODBC 驱动程序和 pyodbc

```bash
pip install pyodbc
```
可在[此处](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)找到有关安装 Python 和数据库通信库的更多详细信息。

### <a name="example"></a>示例

连接到 SQL 数据库并选择表中的所有记录。

```python
import pyodbc 

SERVER = 'YOUR_SERVER_NAME.database.windows.net'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_DB_USERNAME'
PASSWORD = 'YOUR_DB_PASSWORD'

DRIVER= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER=' + DRIVER + ';PORT=1433;SERVER=' + SERVER +
    ';PORT=1443;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>管理 API

使用管理 API 在订阅中创建和管理 Azure SQL 数据库资源。 

```bash
pip install azure-mgmt-sql
```

### <a name="example"></a>示例

创建 SQL 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/sql/managementlibrary)

## <a name="samples"></a>示例

* [创建和管理 SQL 数据库][1]    
* [使用 Python 连接和查询数据][2]   

[1]: https://github.com/Azure-Samples/sql-database-python-manage
[2]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python

查看 Azure SQL 数据库示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL)。 