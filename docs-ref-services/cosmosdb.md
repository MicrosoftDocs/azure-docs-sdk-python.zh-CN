---
title: "用于 Python 的 Azure CosmosDB 库"
description: "用于 CosmosDB 的 Python 客户端库的参考文档"
keywords: "Azure, Python, SDK, API, SQL, 数据库, PostGres,CosmosDB, NoSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/11/2017
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 5382779d659f7c85e5ecbd304920e00b78a08a49
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmosdb-libraries-for-python"></a>用于 Python 的 Azure CosmosDB 库

## <a name="overview"></a>概述

在 Python 应用程序中使用 CosmosDB，以便在 NoSQL 数据存储中存储和查询 JSON 文档。

详细了解 [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction)。

## <a name="client-library"></a>客户端库
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>管理库
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>示例

在使用类似于 SQL 的查询接口在 CosmosDB 中查找匹配的文档：

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python DocumentDB client
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
> [了解管理 API](/python/api/overview/azure/cosmosdb/managementlibrary)

## <a name="samples"></a>示例

[使用 Azure Cosmos DB 的 DocumentDB API 开发 Python 应用](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


