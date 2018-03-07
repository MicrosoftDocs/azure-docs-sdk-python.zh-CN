---
title: "用于 Python 的服务总线库"
description: "用于服务总线的 Python 客户端和管理库的参考文档"
keywords: "Azure, Python, SDK, API, 消息传送, pubsub, 发布-订阅, 消息中转站"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 6c0bc66fbe8194b5b8f34ee8e29945b03ba8c242
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="77a3b-104">用于 Python 的服务总线库</span><span class="sxs-lookup"><span data-stu-id="77a3b-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="77a3b-105">概述</span><span class="sxs-lookup"><span data-stu-id="77a3b-105">Overview</span></span>

<span data-ttu-id="77a3b-106">Microsoft Azure 服务总线支持一组基于云的、面向消息的中间件技术，包括可靠的消息队列和持久的发布/订阅消息。</span><span class="sxs-lookup"><span data-stu-id="77a3b-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="77a3b-107">安装库</span><span class="sxs-lookup"><span data-stu-id="77a3b-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="servicebus-queues"></a><span data-ttu-id="77a3b-108">服务总线队列</span><span class="sxs-lookup"><span data-stu-id="77a3b-108">ServiceBus Queues</span></span>
<span data-ttu-id="77a3b-109">服务总线队列是存储队列的替代方法，可能在需要使用推送型传递（使用长轮询）执行更高级的消息功能（较大的消息大小、消息排序、单操作破坏性读取、按计划执行的传送）时十分有用。</span><span class="sxs-lookup"><span data-stu-id="77a3b-109">ServiceBus Queues are an alternative to Storage Queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

<span data-ttu-id="77a3b-110">该服务可以使用共享访问签名身份验证或 ACS 身份验证。</span><span class="sxs-lookup"><span data-stu-id="77a3b-110">The service can use Shared Access Signature authentication, or ACS authentication.</span></span>

<span data-ttu-id="77a3b-111">2014 年 8 月后使用 Azure 门户创建的服务总线命名空间不再支持 ACS 身份验证。</span><span class="sxs-lookup"><span data-stu-id="77a3b-111">Service bus namespaces created using the Azure portal after August 2014 no longer support ACS authentication.</span></span> <span data-ttu-id="77a3b-112">可以使用 Azure SDK 创建与 ACS 兼容的命名空间。</span><span class="sxs-lookup"><span data-stu-id="77a3b-112">You can create ACS compatible namespaces with the Azure SDK.</span></span>

### <a name="shared-access-signature-authentication"></a><span data-ttu-id="77a3b-113">共享访问签名身份验证</span><span class="sxs-lookup"><span data-stu-id="77a3b-113">Shared Access Signature Authentication</span></span>

<span data-ttu-id="77a3b-114">若要使用共享访问签名身份验证，请使用以下代码创建服务总线服务：</span><span class="sxs-lookup"><span data-stu-id="77a3b-114">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a><span data-ttu-id="77a3b-115">ACS 身份验证</span><span class="sxs-lookup"><span data-stu-id="77a3b-115">ACS Authentication</span></span>

<span data-ttu-id="77a3b-116">若要使用 ACS 身份验证，请使用以下代码创建服务总线服务：</span><span class="sxs-lookup"><span data-stu-id="77a3b-116">To use ACS authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a><span data-ttu-id="77a3b-117">发送和接收消息</span><span class="sxs-lookup"><span data-stu-id="77a3b-117">Sending and Receiving Messages</span></span>

<span data-ttu-id="77a3b-118">**create\_queue** 方法可用于确保队列存在：</span><span class="sxs-lookup"><span data-stu-id="77a3b-118">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="77a3b-119">然后可以调用 **send\_queue\_message** 方法将消息插入到队列：</span><span class="sxs-lookup"><span data-stu-id="77a3b-119">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="77a3b-120">然后可以调用 **send\_queue\_message_batch** 方法来一次发送多条消息：</span><span class="sxs-lookup"><span data-stu-id="77a3b-120">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="77a3b-121">然后可以调用 **receive\_queue\_message** 方法将消息取消排队。</span><span class="sxs-lookup"><span data-stu-id="77a3b-121">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a><span data-ttu-id="77a3b-122">服务总线主题</span><span class="sxs-lookup"><span data-stu-id="77a3b-122">ServiceBus Topics</span></span>

<span data-ttu-id="77a3b-123">服务总线主题是基于服务总线队列的抽象，服务总线队列可使发布/订阅方案易于实现。</span><span class="sxs-lookup"><span data-stu-id="77a3b-123">ServiceBus topics are an abstraction on top of ServiceBus Queues that make pub/sub scenarios easy to implement.</span></span>

<span data-ttu-id="77a3b-124">**create\_topic** 方法可用于创建服务器端主题：</span><span class="sxs-lookup"><span data-stu-id="77a3b-124">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="77a3b-125">**send\_topic\_message** 方法可用于向主题发送一个消息：</span><span class="sxs-lookup"><span data-stu-id="77a3b-125">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="77a3b-126">**send\_topic\_message_batch** 方法可用于一次发送多个消息：</span><span class="sxs-lookup"><span data-stu-id="77a3b-126">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="77a3b-127">请考虑，在 Python 3 中，字符串消息将进行 utf-8 编码，并且你必须在 Python 2 中自行管理你的编码。</span><span class="sxs-lookup"><span data-stu-id="77a3b-127">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="77a3b-128">然后，客户端可以创建订阅并开始通过调用 **create\_subscription** 方法（后跟 **receive\_subscription\_message** 方法）使用消息。</span><span class="sxs-lookup"><span data-stu-id="77a3b-128">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="77a3b-129">请注意，创建订阅之前发送的任何消息均不会被收到。</span><span class="sxs-lookup"><span data-stu-id="77a3b-129">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a><span data-ttu-id="77a3b-130">事件中心</span><span class="sxs-lookup"><span data-stu-id="77a3b-130">Event Hub</span></span>

<span data-ttu-id="77a3b-131">使用事件中心能够以高呑吐量从一组不同的设备和服务收集事件流。</span><span class="sxs-lookup"><span data-stu-id="77a3b-131">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="77a3b-132">**create\_event\_hub** 方法可用于创建事件中心：</span><span class="sxs-lookup"><span data-stu-id="77a3b-132">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="77a3b-133">若要发送事件，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="77a3b-133">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="77a3b-134">事件内容是事件消息或包含多个消息的 JSON 编码字符串。</span><span class="sxs-lookup"><span data-stu-id="77a3b-134">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

## <a name="advanced-features"></a><span data-ttu-id="77a3b-135">高级功能</span><span class="sxs-lookup"><span data-stu-id="77a3b-135">Advanced features</span></span>

### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="77a3b-136">Broker 属性和 User 属性</span><span class="sxs-lookup"><span data-stu-id="77a3b-136">Broker Properties and User Properties</span></span>

<span data-ttu-id="77a3b-137">本部分介绍如何使用[此处](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)定义的 Broker 和 User 属性：</span><span class="sxs-lookup"><span data-stu-id="77a3b-137">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                    broker_properties={'Label': 'M3'},
                    custom_properties={'Priority': 'Medium',
                                        'Customer': 'ABC'}
            )
```
<span data-ttu-id="77a3b-138">可以使用 datetime、int、float 或 boolean</span><span class="sxs-lookup"><span data-stu-id="77a3b-138">You can use datetime, int, float or boolean</span></span>

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
<span data-ttu-id="77a3b-139">出于与此库的旧版本的兼容性原因，`broker_properties` 也可以定义为 JSON 字符串。</span><span class="sxs-lookup"><span data-stu-id="77a3b-139">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="77a3b-140">如果是这种情况，那么你要负责编写有效的 JSON 字符串，因为在发送到 RestAPI 之前，Python 不会执行任何检查。</span><span class="sxs-lookup"><span data-stu-id="77a3b-140">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                    broker_properties = broker_properties
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="77a3b-141">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="77a3b-141">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/management)