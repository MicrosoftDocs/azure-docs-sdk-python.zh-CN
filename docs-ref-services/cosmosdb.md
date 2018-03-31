---
title: 用于 Python 的 Azure Cosmos DB 库
description: 用于 Azure Cosmos DB 的 Python 客户端库的参考文档
keywords: Azure, Python, SDK, API, SQL, 数据库, PostGres, Cosmos DB, NoSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 391b556ece7d818406fa501763814eb7f0d50d22
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-libraries-for-python"></a><span data-ttu-id="479e9-104">用于 Python 的 Azure Cosmos DB 库</span><span class="sxs-lookup"><span data-stu-id="479e9-104">Azure Cosmos DB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="479e9-105">概述</span><span class="sxs-lookup"><span data-stu-id="479e9-105">Overview</span></span>

<span data-ttu-id="479e9-106">在 Python 应用程序中使用 Azure Cosmos DB，以便在 NoSQL 数据存储中存储和查询 JSON 文档。</span><span class="sxs-lookup"><span data-stu-id="479e9-106">Use Azure Cosmos DB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="479e9-107">了解有关 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) 的详细信息。</span><span class="sxs-lookup"><span data-stu-id="479e9-107">Learn more about [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="479e9-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="479e9-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="479e9-109">管理库</span><span class="sxs-lookup"><span data-stu-id="479e9-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="479e9-110">示例</span><span class="sxs-lookup"><span data-stu-id="479e9-110">Example</span></span>

<span data-ttu-id="479e9-111">使用类似于 SQL 的查询接口在 Azure CosmosDB 中查找匹配的文档：</span><span class="sxs-lookup"><span data-stu-id="479e9-111">Find matching documents in Azure CosmosDB using a SQL-like query interface:</span></span>

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
> [<span data-ttu-id="479e9-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="479e9-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a><span data-ttu-id="479e9-113">示例</span><span class="sxs-lookup"><span data-stu-id="479e9-113">Samples</span></span>

[<span data-ttu-id="479e9-114">使用 Azure Cosmos DB 开发 Python 应用</span><span class="sxs-lookup"><span data-stu-id="479e9-114">Develop a Python app using Azure Cosmos DB</span></span>](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


