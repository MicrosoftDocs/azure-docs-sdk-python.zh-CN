---
title: "用于 Python 的 Azure 事件网格库"
description: 
keywords: "Azure, Python, SDK, API, 事件网格"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: a50a203a0733f25f2a88d6f4a43c6bddc388d3e7
ms.sourcegitcommit: 79afc8a1b427e26ecea7bdc0b7b3c898f143360f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2017
---
# <a name="event-grid-libraries-for-python"></a>用于 Python 的事件网格库

## <a name="overview"></a>概述
Azure 事件网格是一种完全托管的智能事件路由服务，允许通过发布-订阅模型来一致地使用事件。

## <a name="management-api"></a>管理 API
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a>示例
以下示例创建自定义主题、订阅主题，并触发事件来查看结果。 RequestBin 是第三方开源工具，用于创建终结点和查看发送到其中的请求。 转到 [RequestBin](https://requestb.in/)，单击“创建 RequestBin”。 复制 bin URL，因为在订阅主题时需要它。

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
> [了解管理 API](/python/api/overview/azure/eventgrid/managementlibrary)

