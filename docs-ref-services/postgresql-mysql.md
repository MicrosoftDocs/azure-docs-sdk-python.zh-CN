---
title: 用于 Python 的 Azure MySQL/PostgreSQL 库
description: ''
keywords: Azure, Python, SDK, API, SQL, 数据库, MySQL, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.openlocfilehash: e3cd84288ad8d49cfcd673506e2db150c02e7918
ms.sourcegitcommit: 16eecfc4ed0e2a8344ce5887327cdf2619ba89e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2018
ms.locfileid: "39189587"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a>用于 Python 的 Azure MySQL/PostgreSQL 库

## <a name="mysql"></a>MySQL

使用 MySQL 管理器和 pyodbc 通过 Python 处理 [Azure MySQL 数据库](/azure/mysql/overview)中存储的资源和数据。

### <a name="client-odbc-driver-and-pyodbc"></a>客户端 ODBC 驱动程序和 pyodbc

建议用于访问 Azure Database for MySQL 的客户端库是 Microsoft [ODBC 驱动程序](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。 使用 ODBC 驱动程序可直接连接到数据库并执行 SQL 语句。

#### <a name="example"></a>示例

连接到 Azure Database for MySQL，并选择销售表中的所有记录。 可以从 Azure 门户获取数据库的 ODBC 连接字符串。

```python
SERVER = 'YOUR_SEVER_NAME' + '.mysql.database.azure.com'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_MYSQL_USERNAME'
PASSWORD = 'YOUR_MYSQL_PASSWORD'

DRIVER = '{MySQL ODBC 5.3 UNICODE Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=3306;SERVER=' + SERVER + ';PORT=3306;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

### <a name="management-api"></a>管理 API

使用管理 API 在订阅中创建和管理 MySQL 资源。

#### <a name="requirements"></a>要求
必须安装用于 Python 的 MySQL 管理库。
```bash
pip install azure-mgmt-rdbms
```

请参阅 [Python SDK 身份验证](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)页，了解有关使用管理客户端获取身份验证凭据的详细信息。

#### <a name="example"></a>示例

创建 MySQL 5.7 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。

```python

from azure.mgmt.rdbms.mysql import MySQLManagementClient
from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE-GROUP_WITH_POSTGRES"
MYSQL_SERVER = "YOUR_DESIRED_MYSQL_SERVER_NAME"
MYSQL_ADMIN_USER = "YOUR_MYSQL_ADMIN_USERNAME"
MYSQL_ADMIN_PASSWORD = "YOUR_MYSQL_ADMIN_PASSOWRD"
LOCATION = "westus"  # example Azure availability zone, should match resource group


client = MySQLManagementClient(credentials=creds,
    subscription_id=SUBSCRIPTION_ID)

server_creation_poller = client.servers.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=MYSQL_SERVER,
    ServerForCreate(
        ServerPropertiesForDefaultCreate(
            administrator_login=MYSQL_ADMIN_USER,
            administrator_login_password=MYSQL_ADMIN_PASSWORD,
            version=ServerVersion.five_full_stop_seven
        ),
        location=LOCATION
    )
)

server = server_creation_poller.result()

# Open access to this server for IPs
rule_creation_poller = client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    MYSQL_SERVER,
    "some_custom_ip_range_whitelist_rule_name",  # Custom firewall rule name
    "123.123.123.123",  # Start ip range
    "167.220.0.235"  # End ip range
)

firewall_rule = rule_creation_poller.result()
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a>PostgreSQL
使用 ODBC 驱动程序和 pyodbc 可直接连接到数据库并执行 SQL 语句。

详细了解 [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/)。

### <a name="client-odbc-driver-and-pyodbc"></a>客户端 ODBC 驱动程序和 pyodbc
建议用于访问 Azure Database for PostgreSQL 的客户端库是 Microsoft [ODBC 驱动程序和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。

#### <a name="example"></a>示例 

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

### <a name="management-api"></a>管理 API
#### <a name="requirements"></a>要求
必须安装用于 Python 的 PostgreSQL 管理库。
```bash
pip install azure-mgmt-rdbms
```

请参阅 [Python SDK 身份验证](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)页，了解有关使用管理客户端获取身份验证凭据的详细信息。

#### <a name="example"></a>示例
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
> [了解管理 API](/python/api/overview/azure/postgresql/mysql/management)