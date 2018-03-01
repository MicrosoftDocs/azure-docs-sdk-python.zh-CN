---
title: "用于 Python 的 Azure SQL 数据库库"
description: "使用 ODBC 驱动程序和 pyodbc 连接到 Azure SQL 数据库，或者使用管理 API 来管理 Azure SQL 实例。"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 01/09/2018
ms.topic: reference
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: 6c442a7a1e639938c993e8c1e6f74bc5e0a730b7
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="azure-sql-database-libraries-for-python"></a>用于 Python 的 Azure SQL 数据库库

## <a name="overview"></a>概述

使用 pyodbc [ODBC 数据库驱动程序](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers)，通过 Python 处理 [Azure SQL 数据库](/azure/sql-database/sql-database-technical-overview)中存储的数据。 查看[快速入门](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python)，了解如何连接到 Azure SQL 数据库、如何使用 Transact-SQL 语句查询数据以及如何开始使用包含 pyodbc 的[示例](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)。

## <a name="install-odbc-driver-and-pyodbc"></a>安装 ODBC 驱动程序和 pyodbc

```bash
pip install pyodbc
```
有关安装 Python 和数据库通信库的更多[详细信息](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。

## <a name="connect-and-execute-a-sql-query"></a>连接并执行 SQL 查询

### <a name="connect-to-a-sql-database"></a>连接到 SQL 数据库

```python
import pyodbc

server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'

cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
```

### <a name="execute-a-sql-query"></a>执行 SQL 查询

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [pyodbc 示例](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a>连接到 ORM

pyodbc 使用其他 ORM（例如 [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) 和 [Django](https://github.com/lionheart/django-pyodbc/)）。 

## <a name="management-apipythonapioverviewazuresqlmanagement"></a>[管理 API](/python/api/overview/azure/sql/management)

使用管理 API 在订阅中创建和管理 Azure SQL 数据库资源。 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a>示例

创建 SQL 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

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
> [了解管理 API](/python/api/overview/azure/sql/management)

