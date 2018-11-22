---
title: 用于 Python 的 Azure 事件网格库
description: ''
keywords: Azure, Python, SDK, API, 事件网格
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: bfaa1908295eb77531e399f1337acdeee512005f
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276831"
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="fca6c-103">用于 Python 的事件网格库</span><span class="sxs-lookup"><span data-stu-id="fca6c-103">Event Grid libraries for Python</span></span>


<span data-ttu-id="fca6c-104">Azure 事件网格是一种完全托管的智能事件路由服务，允许通过发布-订阅模型来一致地使用事件。</span><span class="sxs-lookup"><span data-stu-id="fca6c-104">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

<span data-ttu-id="fca6c-105">[详细了解](/azure/event-grid/overview) Azure 事件网格，通过 [Azure Blob 存储事件教程](/azure/storage/blobs/storage-blob-event-quickstart)来入门。</span><span class="sxs-lookup"><span data-stu-id="fca6c-105">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="publish-sdk"></a><span data-ttu-id="fca6c-106">发布 SDK</span><span class="sxs-lookup"><span data-stu-id="fca6c-106">Publish SDK</span></span>

<span data-ttu-id="fca6c-107">进行身份验证，执行创建和处理操作，然后使用 Azure 事件网格发布 SDK 将事件发布到主题。</span><span class="sxs-lookup"><span data-stu-id="fca6c-107">Authenticate, create, handle, and publish events to topics using the Azure Event Grid publish SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="fca6c-108">安装</span><span class="sxs-lookup"><span data-stu-id="fca6c-108">Installation</span></span> 

<span data-ttu-id="fca6c-109">使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 来安装包：</span><span class="sxs-lookup"><span data-stu-id="fca6c-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-eventgrid
```

### <a name="example"></a><span data-ttu-id="fca6c-110">示例</span><span class="sxs-lookup"><span data-stu-id="fca6c-110">Example</span></span> 

<span data-ttu-id="fca6c-111">以下代码将事件发布到主题。</span><span class="sxs-lookup"><span data-stu-id="fca6c-111">The following code publishes an event to a topic.</span></span> <span data-ttu-id="fca6c-112">可以通过 Azure 门户或 Azure CLI 检索主题键和终结点：</span><span class="sxs-lookup"><span data-stu-id="fca6c-112">You can retrieve the topic key and endpoint through the Azure Portal or through the Azure CLI:</span></span>

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
> [<span data-ttu-id="fca6c-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="fca6c-113">Explore the Client APIs</span></span>](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="fca6c-114">管理 SDK</span><span class="sxs-lookup"><span data-stu-id="fca6c-114">Management SDK</span></span>

<span data-ttu-id="fca6c-115">使用管理 SDK 创建、更新或删除事件网格实例、主题和订阅。</span><span class="sxs-lookup"><span data-stu-id="fca6c-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the management SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="fca6c-116">安装</span><span class="sxs-lookup"><span data-stu-id="fca6c-116">Installation</span></span> 

<span data-ttu-id="fca6c-117">使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 来安装包：</span><span class="sxs-lookup"><span data-stu-id="fca6c-117">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="fca6c-118">示例</span><span class="sxs-lookup"><span data-stu-id="fca6c-118">Example</span></span>

<span data-ttu-id="fca6c-119">以下示例创建自定义主题并让终结点订阅主题。</span><span class="sxs-lookup"><span data-stu-id="fca6c-119">The following creates a custom topic and subscribes an endpoint to the topic.</span></span> <span data-ttu-id="fca6c-120">该代码然后通过 HTTPS 将事件发送到主题。</span><span class="sxs-lookup"><span data-stu-id="fca6c-120">The code then sends an event to the topic through HTTPS.</span></span>
<span data-ttu-id="fca6c-121">RequestBin 是第三方开源工具，用于创建终结点和查看发送到其中的请求。</span><span class="sxs-lookup"><span data-stu-id="fca6c-121">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="fca6c-122">转到 [RequestBin](https://requestb.in/)，单击“创建 RequestBin”。</span><span class="sxs-lookup"><span data-stu-id="fca6c-122">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="fca6c-123">复制 bin URL，因为在订阅主题时需要它。</span><span class="sxs-lookup"><span data-stu-id="fca6c-123">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

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
<span data-ttu-id="fca6c-124">浏览到前面创建的 RequestBin URL 即可查看刚刚发送的事件。</span><span class="sxs-lookup"><span data-stu-id="fca6c-124">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="fca6c-125">清理资源</span><span class="sxs-lookup"><span data-stu-id="fca6c-125">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="fca6c-126">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="fca6c-126">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a><span data-ttu-id="fca6c-127">了解详细信息</span><span class="sxs-lookup"><span data-stu-id="fca6c-127">Learn more</span></span>

[<span data-ttu-id="fca6c-128">使用事件网格 SDK 接收事件</span><span class="sxs-lookup"><span data-stu-id="fca6c-128">Receive events using the Event Grid SDK</span></span>](/azure/event-grid/receive-events)
