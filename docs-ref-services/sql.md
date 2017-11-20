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
# <a name="azure-sql-database-libraries-for-python"></a><span data-ttu-id="9d217-103">用于 Python 的 Azure SQL 数据库库</span><span class="sxs-lookup"><span data-stu-id="9d217-103">Azure SQL Database libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="9d217-104">概述</span><span class="sxs-lookup"><span data-stu-id="9d217-104">Overview</span></span>

<span data-ttu-id="9d217-105">使用 Microsoft ODBC 驱动程序和 pyodbc 通过 Python 处理 [Azure SQL 数据库](/azure/sql-database/sql-database-technical-overview)中存储的数据。</span><span class="sxs-lookup"><span data-stu-id="9d217-105">Work with data stored in [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) from Python with the Microsoft ODBC driver and pyodbc.</span></span> 

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="9d217-106">客户端 ODBC 驱动程序和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="9d217-106">Client ODBC driver and pyodbc</span></span>

```bash
pip install pyodbc
```
<span data-ttu-id="9d217-107">可在[此处](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)找到有关安装 Python 和数据库通信库的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="9d217-107">More details about installing the python and database communication libraries can be found [here](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span>

### <a name="example"></a><span data-ttu-id="9d217-108">示例</span><span class="sxs-lookup"><span data-stu-id="9d217-108">Example</span></span>

<span data-ttu-id="9d217-109">连接到 SQL 数据库并选择表中的所有记录。</span><span class="sxs-lookup"><span data-stu-id="9d217-109">Connect to a SQL database and select all records in a table.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="9d217-110">管理 API</span><span class="sxs-lookup"><span data-stu-id="9d217-110">Management API</span></span>

<span data-ttu-id="9d217-111">使用管理 API 在订阅中创建和管理 Azure SQL 数据库资源。</span><span class="sxs-lookup"><span data-stu-id="9d217-111">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span> 

```bash
pip install azure-mgmt-sql
```

### <a name="example"></a><span data-ttu-id="9d217-112">示例</span><span class="sxs-lookup"><span data-stu-id="9d217-112">Example</span></span>

<span data-ttu-id="9d217-113">创建 SQL 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。</span><span class="sxs-lookup"><span data-stu-id="9d217-113">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

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
> [<span data-ttu-id="9d217-114">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="9d217-114">Explore the Management APIs</span></span>](/python/api/overview/azure/sql/managementlibrary)

## <a name="samples"></a><span data-ttu-id="9d217-115">示例</span><span class="sxs-lookup"><span data-stu-id="9d217-115">Samples</span></span>

* <span data-ttu-id="9d217-116">[创建和管理 SQL 数据库][1]</span><span class="sxs-lookup"><span data-stu-id="9d217-116">[Create and manage SQL databases][1]</span></span>    
* <span data-ttu-id="9d217-117">[使用 Python 连接和查询数据][2]</span><span class="sxs-lookup"><span data-stu-id="9d217-117">[Use Python to connect and query data][2]</span></span>   

[1]: https://github.com/Azure-Samples/sql-database-python-manage
[2]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python

<span data-ttu-id="9d217-118">查看 Azure SQL 数据库示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL)。</span><span class="sxs-lookup"><span data-stu-id="9d217-118">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL) of Azure SQL database samples.</span></span> 