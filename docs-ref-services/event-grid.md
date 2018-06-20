---
title: 用于 Python 的 Azure 事件网格库
description: ''
keywords: Azure, Python, SDK, API, 事件网格
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: e68504b3ba5834a145af1231eacc076e424a2256
ms.sourcegitcommit: 560362db0f65307c8b02b7b7ad8642b5c4aa6294
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2018
ms.locfileid: "33901422"
---
# <a name="event-grid-libraries-for-python"></a>用于 Python 的事件网格库


Azure 事件网格是一种完全托管的智能事件路由服务，允许通过发布-订阅模型来一致地使用事件。

[详细了解](/azure/event-grid/overview) Azure 事件网格，通过 [Azure Blob 存储事件教程](/azure/storage/blobs/storage-blob-event-quickstart)来入门。 

## <a name="publish-sdk"></a>发布 SDK

进行身份验证，执行创建和处理操作，然后使用 Azure 事件网格发布 SDK 将事件发布到主题。

### <a name="installation"></a>安装 

使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 来安装包：

```bash
pip install azure-eventgrid
```

### <a name="example"></a>示例 

以下代码将事件发布到主题。 可以通过 Azure 门户或 Azure CLI 检索主题键和终结点：

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

```python
from datetime import datetime
from azure.eventgrid import EventGridClient
from msrest.authentication import TopicCredentials

def publish_event(self):

        credentials = TopicCredentials(
            self.settings.EVENT_GRID_KEY
        )
        event_grid_client = EventGridClient(credentials)
        event_grid_client.publish_events(
            "your-endpoint-here",
            events=[{
                'id' : "dbf93d79-3859-4cac-8055-51e3b6b54bea",
                'subject' : "Sample subject",
                'data': {
                    'key': 'Sample Data'
                },
                'event_type': 'SampleEventType',
                'event_time': datetime(2018, 5, 2),
                'data_version': 1
            }]
        )
```

> [!div class="nextstepaction"]
> [了解客户端 API](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a>管理 SDK

使用管理 SDK 创建、更新或删除事件网格实例、主题和订阅。

### <a name="installation"></a>安装 

使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 来安装包：

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a>示例

以下示例创建自定义主题并让终结点订阅主题。 该代码然后通过 HTTPS 将事件发送到主题。
RequestBin 是第三方开源工具，用于创建终结点和查看发送到其中的请求。 转到 [RequestBin](https://requestb.in/)，单击“创建 RequestBin”。 复制 bin URL，因为在订阅主题时需要它。

```python
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.eventgrid import EventGridManagementClient
import requests

RESOURCE_GROUP_NAME = 'gridResourceGroup'
TOPIC_NAME = 'gridTopic1234'
LOCATION = 'westus2'
SUBSCRIPTION_ID = 'YOUR_SUBSCRIPTION_ID'
SUBSCRIPTION_NAME = 'gridSubscription'
REQUEST_BIN_URL = 'YOUR_REQUEST_BIN_URL'

# create resource group
resource_client = ResourceManagementClient(credentials, SUBSCRIPTION_ID)
resource_client.resource_groups.create_or_update(
    RESOURCE_GROUP_NAME,
    {
        'location': LOCATION
    }
)

event_client = EventGridManagementClient(credentials, SUBSCRIPTION_ID)

# create a custom topic
event_client.topics.create_or_update(RESOURCE_GROUP_NAME, TOPIC_NAME, LOCATION)

# subscribe to a topic
scope = '/subscriptions/'+SUBSCRIPTION_ID+'/resourceGroups/'+RESOURCE_GROUP_NAME+'/providers/Microsoft.EventGrid/topics/'+TOPIC_NAME
event_client.event_subscriptions.create(scope, SUBSCRIPTION_NAME,
    {
        'destination': {
            'endpoint_url': REQUEST_BIN_URL
        }
    }
)

# send an event to topic
# get endpoint url
url = event_client.event_subscriptions.get_full_url(scope, SUBSCRIPTION_NAME).endpoint_url
# get key
key = event_client.topics.list_shared_access_keys(RESOURCE_GROUP_NAME,TOPIC_NAME).key1
headers = {'aeg-sas-key': key}
s = requests.get('https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json')
r = requests.post(url, data=s, headers=headers)
print(r.status_code)
print(r.content)
```
浏览到前面创建的 RequestBin URL 即可查看刚刚发送的事件。

清理资源
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a>了解详细信息

[使用事件网格 SDK 接收事件](/azure/event-grid/receive-events)