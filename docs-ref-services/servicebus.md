---
title: 用于 Python 的服务总线库
description: 用于服务总线的 Python 客户端和管理库的参考文档
keywords: Azure, Python, SDK, API, 消息传送, pubsub, 发布-订阅, 消息中转站
author: annatisch
ms.author: antisch
manager: mayurid
ms.date: 01/15/2019
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 014f34b6ba3d3a2a8ce15afcd826195d6051f98a
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376768"
---
# <a name="service-bus-libraries-for-python"></a>用于 Python 的服务总线库

Microsoft Azure 服务总线支持一组基于云的、面向消息的中间件技术，包括可靠的消息队列和持久的发布/订阅消息。

* [SDK 源代码](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [SDK 参考文档](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a>v0.50.0 中有哪些新增功能？

从版本 0.50.0 开始，可以通过新的基于 AMQP 的 API 来发送和接收消息。 此更新涉及**中断性变更**。

请阅读[从 v0.21.1 迁移到 v0.50.0](#migration-from-v0211-to-v0500)，确定目前是否适合升级。

新的基于 AMQP 的 API 提供改进的消息传递可靠性、性能以及将来的扩展功能支持。
新 API 还提供基于 asyncio 的异步操作支持，可以通过异步操作发送、接收和处理消息。

有关基于 HTTP 的旧操作的文档，请参阅[使用旧 API 的基于 HTTP 的操作](#using-http-based-operations-of-the-legacy-api)。

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建免费帐户](https://azure.microsoft.com/free/)
* Azure [服务总线命名空间和管理凭据](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)
* [Python 2.7-3.7](https://www.python.org/downloads/)

## <a name="installation"></a>安装

```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a>连接到 Azure 服务总线

### <a name="get-credentials"></a>获取凭据

使用下面的 Azure CLI 代码片段为环境变量填充服务总线连接字符串（也可在 Azure 门户中找到此值）。 此代码片段已针对 Bash shell 格式化。

```Bash
RES_GROUP=<resource-group-name>
NAMESPACE=<servicebus-namespace>

export SB_CONN_STR=$(az servicebus namespace authorization-rule keys list \
 --resource-group $RES_GROUP \
 --namespace-name $NAMESPACE \
 --name RootManageSharedAccessKey \
 --query primaryConnectionString \
 --output tsv)
```

### <a name="create-client"></a>创建客户端

填充 `SB_CONN_STR` 环境变量后，即可创建 ServiceBusClient。

```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

如果希望使用异步操作，请使用 `azure.servicebus.aio` 命名空间。

```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

## <a name="service-bus-queues"></a>服务总线队列

服务总线队列是存储队列的替代方法，可能在需要使用推送型传递（使用长轮询）执行更高级的消息功能（较大的消息大小、消息排序、单操作破坏性读取、按计划执行的传送）时十分有用。

### <a name="create-queue"></a>创建队列

这样会在服务总线命名空间中创建新的队列。 如果命名空间中已经存在相同名称的队列，则会引发错误。

```python
sb_client.create_queue("MyQueue")
```

也可指定用于配置队列行为的可选参数。

```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a>获取队列客户端

可以使用 QueueClient 通过队列发送和接收消息，以及执行其他操作。

```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a>发送消息

此队列客户端可以一次发送一条或多条消息：

```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```

每次调用 QueueClient.send 都会创建新的服务连接。 若要将同一连接重复用于多个发送调用，可以打开一个发送程序：

```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```

如果使用异步客户端，则上述操作会使用异步语法：

```python
from azure.servicebus.aio import Message

message = Message("Hello World")
await queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
async with queue_client.get_sender() as sender:
    await sender.send(message_one)
    await sender.send(message_two)
```

### <a name="receiving-messages"></a>接收消息

可以从队列以连续迭代器的方式接收消息。 进行消息接收的默认模式是 [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read)，此模式要求显式完成每条消息，以便将其从队列中删除。

```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```

在迭代器运行期间，服务连接将始终保持打开状态。 如果发现自己只对消息流进行了部分迭代，则应在 `with` 语句中运行接收器，确保连接关闭：

```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
如果使用异步客户端，则上述操作会使用异步语法：

```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a>服务总线主题和订阅

服务总线主题和订阅是基于服务总线队列的抽象，该队列以“发布/订阅”模式提供一对多的通信形式。 消息发送到一个主题并传送到一个或多个关联订阅，这可以用来扩大接收方的数目。

### <a name="create-topic"></a>创建主题

这样会在服务总线命名空间中创建新的主题。 如果已经存在相同名称的主题，则会引发错误。

```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a>获取主题客户端

可以使用 TopicClient 将消息发送到主题，以及执行其他操作。

```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a>创建订阅

这样会在服务总线命名空间中为指定主题创建新的订阅。

```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a>获取订阅客户端

可以使用 SubscriptionClient 通过主题接收消息，以及执行其他操作。

```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a>从 v0.21.1 迁移到 v0.50.0

版本 0.50.0 中引入了主要的中断性变更。 原始的基于 HTTP 的 API 在 v0.50.0 中仍可用 - 不过，它现在存在于新的命名空间：`azure.servicebus.control_client`。

### <a name="should-i-upgrade"></a>是否应该升级？

与 v0.21.1 相比，新包 (v0.50.0) 没有在基于 HTTP 的操作方面提供任何改进。 基于 HTTP 的 API 未做更改，只是现在存在于新的命名空间。 因此，如果只是希望使用基于 HTTP 的操作（`create_queue`、`delete_queue` 等），则现在升级没有额外的好处。

### <a name="how-do-i-migrate-my-code-to-the-new-version"></a>如何将代码迁移到新版本？

针对 v0.21.0 编写的代码可以移植到版本 0.50.0，只需更改导入命名空间即可：

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a>使用旧 API 的基于 HTTP 的操作

以下文档介绍旧版 API，应该适用于那些希望将现有代码移植到 v0.50.0 但不做任何其他更改的用户。 此参考也可用作那些使用 v0.21.1 的用户的指南。
对于那些编写新代码的用户，建议使用上面介绍的新 API。

### <a name="service-bus-queues"></a>服务总线队列

#### <a name="shared-access-signature-sas-authentication"></a>共享访问签名 (SAS) 身份验证

若要使用共享访问签名身份验证，请使用以下代码创建服务总线服务：

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a>访问控制服务 (ACS) 身份验证

新的服务总线命名空间不支持 ACS。 建议[将应用程序迁移到 SAS 身份验证](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)。 若要在旧版服务总线命名空间中使用 ACS 身份验证，请使用以下命令创建 ServiceBusService：

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```

#### <a name="sending-and-receiving-messages"></a>发送和接收消息

**create\_queue** 方法可用于确保队列存在：

```python
sbs.create_queue('taskqueue')
```

然后可以调用 **send\_queue\_message** 方法将消息插入到队列：

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```

然后可以调用 **send\_queue\_message_batch** 方法来一次发送多条消息：

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```

然后可以调用 **receive\_queue\_message** 方法将消息取消排队。

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a>服务总线主题

**create\_topic** 方法可用于创建服务器端主题：

```python
sbs.create_topic('taskdiscussion')
```

**send\_topic\_message** 方法可用于向主题发送一个消息：

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

**send\_topic\_message_batch** 方法可用于一次发送多条消息：

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

请考虑，在 Python 3 中，字符串消息将进行 utf-8 编码，并且你必须在 Python 2 中自行管理你的编码。

然后，客户端可以创建订阅并开始通过调用 **create\_subscription** 方法（后跟 **receive\_subscription\_message** 方法）使用消息。 请注意，创建订阅之前发送的任何消息均不会被收到。

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a>事件中心

使用事件中心能够以高呑吐量从一组不同的设备和服务收集事件流。

**create\_event\_hub** 方法可用于创建事件中心：

```python
sbs.create_event_hub('myhub')
```

若要发送事件，请执行以下操作：

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```

事件内容是事件消息或包含多个消息的 JSON 编码字符串。

### <a name="advanced-features"></a>高级功能

#### <a name="broker-properties-and-user-properties"></a>Broker 属性和 User 属性

本部分介绍如何使用[此处](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)定义的 Broker 和 User 属性：

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```

可以使用 datetime、int、float 或 boolean

```python
props = {'hello': 'world',
         'number': 42,
         'active': True,
         'deceased': False,
         'large': 8555111000,
         'floating': 3.14,
         'dob': datetime(2011, 12, 14),
         'double_quote_message': 'This "should" work fine',
         'quote_message': "This 'should' work fine"}
sent_msg = Message(b'message with properties', custom_properties=props)
```

出于与此库的旧版本的兼容性原因，`broker_properties` 也可以定义为 JSON 字符串。
如果是这种情况，那么你要负责编写有效的 JSON 字符串，因为在发送到 RestAPI 之前，Python 不会执行任何检查。

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a>后续步骤

* [服务总线文档](https://docs.microsoft.com/azure/service-bus-messaging)
* [SDK 源代码](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [SDK 参考文档](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [其他示例](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
