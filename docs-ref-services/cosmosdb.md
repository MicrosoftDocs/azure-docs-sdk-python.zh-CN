---
title: 用于 Python 的 Azure Cosmos DB 库
description: 用于 Azure Cosmos DB 的 Python 客户端库的参考文档
keywords: Azure, Python, SDK, API, SQL, 数据库, PostGres, Cosmos DB, NoSQL
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: bb5e2af6a28d9543cce0c1e80fab1575b78f8cfa
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534318"
---
# <a name="azure-cosmos-db-libraries-for-python"></a>用于 Python 的 Azure Cosmos DB 库

## <a name="overview"></a>概述

在 Python 应用程序中使用 Azure Cosmos DB，以便在 NoSQL 数据存储中存储和查询 JSON 文档。

了解有关 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) 的详细信息。

## <a name="client-library"></a>客户端库
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>管理库
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>示例

使用类似于 SQL 的查询接口在 Azure CosmosDB 中查找匹配的文档：

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python Azure Cosmos DB client
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
# Create a database
db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })

# Create collection options
options = {
    'offerEnableRUPerMinuteThroughput': True,
    'offerVersion': "V2",
    'offerThroughput': 400
}

# Create a collection
collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)

# Create some documents
document1 = client.CreateDocument(collection['_self'],
    { 
        'id': 'server1',
        'Web Site': 0,
        'Cloud Service': 0,
        'Virtual Machine': 0,
        'name': 'some' 
    })

# Query them in SQL
query = { 'query': 'SELECT * FROM server s' }    

options = {} 
options['enableCrossPartitionQuery'] = True
options['maxItemCount'] = 2

result_iterable = client.QueryDocuments(collection['_self'], query, options)
results = list(result_iterable)

print(results)
```
> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a>示例

* [开发一个 Python 应用来访问和管理存储在 Azure Cosmos DB SQL API 帐户中的数据](https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git)

* [开发一个 Python 应用来访问和管理存储在 Azure Cosmos DB MongoDB API 帐户中的数据](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git)

* [开发一个 Python 应用来访问和管理存储在 Azure Cosmos DB Gremlin API 帐户中的数据](https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git)

* [开发一个 Python 应用来访问和管理存储在 Azure Cosmos DB Cassandra API 帐户中的数据](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git)

* [开发一个 Python 应用来访问和管理存储在 Azure Cosmos DB 表 API 帐户中的数据](https://github.com/Azure-Samples/storage-python-getting-started.git)


