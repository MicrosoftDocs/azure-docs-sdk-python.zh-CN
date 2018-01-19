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
ms.openlocfilehash: baa0e53a77d18dc93241135b5b0fecff5786114c
ms.sourcegitcommit: ab96bcebe9d5bfa5f32ec5a61b79bd7483fadcad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2018
---
# <a name="azure-sql-database-libraries-for-python"></a><span data-ttu-id="4fa17-103">用于 Python 的 Azure SQL 数据库库</span><span class="sxs-lookup"><span data-stu-id="4fa17-103">Azure SQL Database libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="4fa17-104">概述</span><span class="sxs-lookup"><span data-stu-id="4fa17-104">Overview</span></span>

<span data-ttu-id="4fa17-105">使用 pyodbc [ODBC 数据库驱动程序](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers)，通过 Python 处理 [Azure SQL 数据库](/azure/sql-database/sql-database-technical-overview)中存储的数据。</span><span class="sxs-lookup"><span data-stu-id="4fa17-105">Work with data stored in [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) from Python with the pyodbc [ODBC database driver](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers).</span></span> <span data-ttu-id="4fa17-106">查看[快速入门](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python)，了解如何连接到 Azure SQL 数据库、如何使用 Transact-SQL 语句查询数据以及如何开始使用包含 pyodbc 的[示例](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)。</span><span class="sxs-lookup"><span data-stu-id="4fa17-106">View our [quickstart](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) on connecting to an Azure SQL database and using Transact-SQL statements to query data and getting started [sample](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) with pyodbc.</span></span>

## <a name="install-odbc-driver-and-pyodbc"></a><span data-ttu-id="4fa17-107">安装 ODBC 驱动程序和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="4fa17-107">Install ODBC driver and pyodbc</span></span>

```bash
pip install pyodbc
```
<span data-ttu-id="4fa17-108">有关安装 Python 和数据库通信库的更多[详细信息](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。</span><span class="sxs-lookup"><span data-stu-id="4fa17-108">More [details](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) about installing the python and database communication libraries.</span></span>

## <a name="connect-and-execute-a-sql-query"></a><span data-ttu-id="4fa17-109">连接并执行 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="4fa17-109">Connect and execute a SQL query</span></span>

### <a name="connect-to-a-sql-database"></a><span data-ttu-id="4fa17-110">连接到 SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="4fa17-110">Connect to a SQL database</span></span>

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

### <a name="execute-a-sql-query"></a><span data-ttu-id="4fa17-111">执行 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="4fa17-111">Execute a SQL query</span></span>

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="4fa17-112">pyodbc 示例</span><span class="sxs-lookup"><span data-stu-id="4fa17-112">pyodbc sample</span></span>](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a><span data-ttu-id="4fa17-113">连接到 ORM</span><span class="sxs-lookup"><span data-stu-id="4fa17-113">Connecting to ORMs</span></span>

<span data-ttu-id="4fa17-114">pyodbc 使用其他 ORM（例如 [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) 和 [Django](https://github.com/lionheart/django-pyodbc/)）。</span><span class="sxs-lookup"><span data-stu-id="4fa17-114">pyodbc works with other ORMs such as [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) and [Django](https://github.com/lionheart/django-pyodbc/).</span></span> 

## <a name="management-apipythonapioverviewazuresqlmanagementlibrary"></a>[<span data-ttu-id="4fa17-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="4fa17-115">Management API</span></span>](/python/api/overview/azure/sql/managementlibrary)

<span data-ttu-id="4fa17-116">使用管理 API 在订阅中创建和管理 Azure SQL 数据库资源。</span><span class="sxs-lookup"><span data-stu-id="4fa17-116">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span> 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="4fa17-117">示例</span><span class="sxs-lookup"><span data-stu-id="4fa17-117">Example</span></span>

<span data-ttu-id="4fa17-118">创建 SQL 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。</span><span class="sxs-lookup"><span data-stu-id="4fa17-118">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

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
> [<span data-ttu-id="4fa17-119">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="4fa17-119">Explore the Management APIs</span></span>](/python/api/overview/azure/sql/managementlibrary)

