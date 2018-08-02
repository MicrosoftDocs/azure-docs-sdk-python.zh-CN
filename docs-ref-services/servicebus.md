---
title: 用于 Python 的服务总线库
description: 用于服务总线的 Python 客户端和管理库的参考文档
keywords: Azure, Python, SDK, API, 消息传送, pubsub, 发布-订阅, 消息中转站
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 02c172ff1a54d060c6af36a5a5daa5dcbff8795c
ms.sourcegitcommit: e35ec475d4b9d8061d0528a93aa8e1c4b7db6c0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2018
ms.locfileid: "39418952"
---
# <a name="service-bus-libraries-for-python"></a>用于 Python 的服务总线库

## <a name="overview"></a>概述

Microsoft Azure 服务总线支持一组基于云的、面向消息的中间件技术，包括可靠的消息队列和持久的发布/订阅消息。 

## <a name="install-the-libraries"></a>安装库
```bash
pip install azure-servicebus
```

## <a name="servicebus-queues"></a>服务总线队列
服务总线队列是存储队列的替代方法，可能在需要使用推送型传递（使用长轮询）执行更高级的消息功能（较大的消息大小、消息排序、单操作破坏性读取、按计划执行的传送）时十分有用。

该服务可以使用共享访问签名身份验证或 ACS 身份验证。

2014 年 8 月后使用 Azure 门户创建的服务总线命名空间不再支持 ACS 身份验证。 可以使用 Azure SDK 创建与 ACS 兼容的命名空间。

### <a name="shared-access-signature-authentication"></a>共享访问签名身份验证

若要使用共享访问签名身份验证，请使用以下代码创建服务总线服务：

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a>ACS 身份验证

若要使用 ACS 身份验证，请使用以下代码创建服务总线服务：

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a>发送和接收消息

**create\_queue** 方法可用于确保队列存在：

```python
sbs.create_queue('taskqueue')
```
然后可以调用 **send\_queue\_message** 方法将消息插入到队列：

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
然后可以调用 **send\_queue\_message_batch** 方法来一次发送多条消息：

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
然后可以调用 **receive\_queue\_message** 方法将消息取消排队。

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a>服务总线主题

服务总线主题是基于服务总线队列的抽象，服务总线队列可使发布/订阅方案易于实现。

**create\_topic** 方法可用于创建服务器端主题：

```python
sbs.create_topic('taskdiscussion')
```
**send\_topic\_message** 方法可用于向主题发送一个消息：

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

**send\_topic\_message_batch** 方法可用于一次发送多个消息：

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

请考虑，在 Python 3 中，字符串消息将进行 utf-8 编码，并且你必须在 Python 2 中自行管理你的编码。

然后，客户端可以创建订阅并开始通过调用 **create\_subscription** 方法（后跟 **receive\_subscription\_message** 方法）使用消息。 请注意，创建订阅之前发送的任何消息均不会被收到。

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a>事件中心

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

## <a name="advanced-features"></a>高级功能

### <a name="broker-properties-and-user-properties"></a>Broker 属性和 User 属性

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

> [!div class="nextstepaction"]
> [了解管理 API](/python/api/overview/azure/servicebus/management)
