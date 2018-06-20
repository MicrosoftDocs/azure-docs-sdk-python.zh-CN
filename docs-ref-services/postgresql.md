---
title: 用于 Python 的 Azure PostgreSQL 库
description: ''
keywords: Azure, Python, SDK, API, SQL, 数据库, Postgres, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: cad5995072d5040764986765d9a900f46f5141ec
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478930"
---
#<a name="azure-postgresql-libraries-for-python"></a>用于 Python 的 Azure PostgreSQL 库

## <a name="overview"></a>概述
使用 ODBC 驱动程序和 pyodbc 可直接连接到数据库并执行 SQL 语句。

详细了解 [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/)。

## <a name="client-odbc-driver-and-pyodbc"></a>客户端 ODBC 驱动程序和 pyodbc
建议用于访问 Azure Database for PostgreSQL 的客户端库是 Microsoft [ODBC 驱动程序和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。

### <a name="example"></a>示例 

连接到 Azure Database for PostgreSQL，并选择 `SALES` 表中的所有记录。 可以从 Azure 门户获取数据库的 ODBC 连接字符串。

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

## <a name="management-api"></a>管理 API
### <a name="requirements"></a>要求
必须安装用于 Python 的 PostgreSQL 管理库。
```bash
pip install azure-mgmt-rdbms
```

请参阅 [Python SDK 身份验证](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)页，了解有关使用管理客户端获取身份验证凭据的详细信息。

### <a name="example"></a>示例
此示例在现有的 Postgres 服务器上创建新的 Postgres 数据库。
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
> [了解管理 API](/python/api/overview/azure/postgresql/management)

