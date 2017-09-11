---
title: "用于 Python 的 Azure PostgreSQL 库"
description: 
keywords: "Azure, Python, SDK, API, SQL, 数据库, Postgres, PostgreSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: e184efc276fb4e6d86504ab44e47340ce72e7006
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
#<a name="azure-postgresql-libraries-for-python"></a><span data-ttu-id="819e1-103">用于 Python 的 Azure PostgreSQL 库</span><span class="sxs-lookup"><span data-stu-id="819e1-103">Azure PostgreSQL libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="819e1-104">概述</span><span class="sxs-lookup"><span data-stu-id="819e1-104">Overview</span></span>
<span data-ttu-id="819e1-105">使用 ODBC 驱动程序和 pyodbc 可直接连接到数据库并执行 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="819e1-105">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="819e1-106">详细了解 [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/)。</span><span class="sxs-lookup"><span data-stu-id="819e1-106">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="819e1-107">客户端 ODBC 驱动程序和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="819e1-107">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="819e1-108">建议用于访问 Azure Database for PostgreSQL 的客户端库是 Microsoft [ODBC 驱动程序和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。</span><span class="sxs-lookup"><span data-stu-id="819e1-108">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

### <a name="example"></a><span data-ttu-id="819e1-109">示例</span><span class="sxs-lookup"><span data-stu-id="819e1-109">Example</span></span> 

<span data-ttu-id="819e1-110">连接到 Azure Database for PostgreSQL，并选择 `SALES` 表中的所有记录。</span><span class="sxs-lookup"><span data-stu-id="819e1-110">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="819e1-111">可以从 Azure 门户获取数据库的 ODBC 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="819e1-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

```python
import pyodbc

SERVER = 'YOUR_SERVER_NAME.postgres.database.azure.com'
DATABASE = 'YOUR_DB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

DRIVER = '{PostgreSQL ODBC Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=5432;SERVER=' + SERVER +
    ';PORT=5432;DATABASE=' + DATABASE + ';UID=' + USERNAME +
    ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES" # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a><span data-ttu-id="819e1-112">管理 API</span><span class="sxs-lookup"><span data-stu-id="819e1-112">Management API</span></span>
### <a name="requirements"></a><span data-ttu-id="819e1-113">要求</span><span class="sxs-lookup"><span data-stu-id="819e1-113">Requirements</span></span>
<span data-ttu-id="819e1-114">必须安装用于 Python 的 PostgreSQL 管理库。</span><span class="sxs-lookup"><span data-stu-id="819e1-114">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="819e1-115">请参阅 [Python SDK 身份验证](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)页，了解有关使用管理客户端获取身份验证凭据的详细信息。</span><span class="sxs-lookup"><span data-stu-id="819e1-115">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="819e1-116">示例</span><span class="sxs-lookup"><span data-stu-id="819e1-116">Example</span></span>
<span data-ttu-id="819e1-117">此示例在现有的 Postgres 服务器上创建新的 Postgres 数据库。</span><span class="sxs-lookup"><span data-stu-id="819e1-117">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
```python
from azure.mgtm.rdbms.postgresql import PostgreSQLManagementClient

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE_GROUP_WITH_POSTGRES"
POSTGRES_SERVER = "YOUR_POSTGRES_SERVER_NAME"
DB_NAME = "YOUR_DESIRED_DATABASE_NAME"

client = PostgreSQLManagementClient(credentials, SUBSCRIPTION_ID)

db_creation_poller = client.databases.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=POSTGRES_SERVER, database_name=DB_NAME)
db = db_creation_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="819e1-118">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="819e1-118">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/managementlibrary)

