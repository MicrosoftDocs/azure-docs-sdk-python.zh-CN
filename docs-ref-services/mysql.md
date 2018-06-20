---
title: 用于 Python 的 Azure MySQL 库
description: ''
keywords: Azure, Python, SDK, API, SQL, 数据库, MySQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: mysql
ms.openlocfilehash: f03134bfddfabc426cbcaf4d98ef86d14038861f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479180"
---
# <a name="azure-mysql-libraries-for-python"></a><span data-ttu-id="143ca-103">用于 Python 的 Azure MySQL 库</span><span class="sxs-lookup"><span data-stu-id="143ca-103">Azure MySQL libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="143ca-104">概述</span><span class="sxs-lookup"><span data-stu-id="143ca-104">Overview</span></span>

<span data-ttu-id="143ca-105">使用 MySQL 管理器和 pyodbc 通过 Python 处理 [Azure MySQL 数据库](/azure/mysql/overview)中存储的资源和数据。</span><span class="sxs-lookup"><span data-stu-id="143ca-105">Work with resources and data stored in [Azure MySQL Database](/azure/mysql/overview) from python with the MySQL manager and pyodbc.</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="143ca-106">客户端 ODBC 驱动程序和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="143ca-106">Client ODBC driver and pyodbc</span></span>

<span data-ttu-id="143ca-107">建议用于访问 Azure Database for MySQL 的客户端库是 Microsoft [ODBC 驱动程序](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。</span><span class="sxs-lookup"><span data-stu-id="143ca-107">The recommended client library for accessing Azure Database for MySQL is the Microsoft [ODBC driver](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span> <span data-ttu-id="143ca-108">使用 ODBC 驱动程序可直接连接到数据库并执行 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="143ca-108">Use the ODBC driver to connect to the database and execute SQL statements directly.</span></span>

### <a name="example"></a><span data-ttu-id="143ca-109">示例</span><span class="sxs-lookup"><span data-stu-id="143ca-109">Example</span></span>

<span data-ttu-id="143ca-110">连接到 Azure Database for MySQL，并选择销售表中的所有记录。</span><span class="sxs-lookup"><span data-stu-id="143ca-110">Connect to a Azure Database for MySQL and select all records in the sales table.</span></span> <span data-ttu-id="143ca-111">可以从 Azure 门户获取数据库的 ODBC 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="143ca-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="143ca-112">管理 API</span><span class="sxs-lookup"><span data-stu-id="143ca-112">Management API</span></span>

<span data-ttu-id="143ca-113">使用管理 API 在订阅中创建和管理 MySQL 资源。</span><span class="sxs-lookup"><span data-stu-id="143ca-113">Create and manage MySQL resources in your subscription with the management API.</span></span>

### <a name="requirements"></a><span data-ttu-id="143ca-114">要求</span><span class="sxs-lookup"><span data-stu-id="143ca-114">Requirements</span></span>
<span data-ttu-id="143ca-115">必须安装用于 Python 的 MySQL 管理库。</span><span class="sxs-lookup"><span data-stu-id="143ca-115">You must install the MySQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="143ca-116">请参阅 [Python SDK 身份验证](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)页，了解有关使用管理客户端获取身份验证凭据的详细信息。</span><span class="sxs-lookup"><span data-stu-id="143ca-116">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="143ca-117">示例</span><span class="sxs-lookup"><span data-stu-id="143ca-117">Example</span></span>

<span data-ttu-id="143ca-118">创建 MySQL 5.7 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。</span><span class="sxs-lookup"><span data-stu-id="143ca-118">Create a MySQL 5.7 Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

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
> [<span data-ttu-id="143ca-119">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="143ca-119">Explore the Management APIs</span></span>](/python/api/overview/azure/mysql/management)