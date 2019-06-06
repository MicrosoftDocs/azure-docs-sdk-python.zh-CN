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
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="6c373-104">用于 Python 的服务总线库</span><span class="sxs-lookup"><span data-stu-id="6c373-104">Service Bus libraries for Python</span></span>

<span data-ttu-id="6c373-105">Microsoft Azure 服务总线支持一组基于云的、面向消息的中间件技术，包括可靠的消息队列和持久的发布/订阅消息。</span><span class="sxs-lookup"><span data-stu-id="6c373-105">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span>

* [<span data-ttu-id="6c373-106">SDK 源代码</span><span class="sxs-lookup"><span data-stu-id="6c373-106">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="6c373-107">SDK 参考文档</span><span class="sxs-lookup"><span data-stu-id="6c373-107">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a><span data-ttu-id="6c373-108">v0.50.0 中有哪些新增功能？</span><span class="sxs-lookup"><span data-stu-id="6c373-108">What's new in v0.50.0?</span></span>

<span data-ttu-id="6c373-109">从版本 0.50.0 开始，可以通过新的基于 AMQP 的 API 来发送和接收消息。</span><span class="sxs-lookup"><span data-stu-id="6c373-109">As of version 0.50.0 a new AMQP-based API is available for sending and receiving messages.</span></span> <span data-ttu-id="6c373-110">此更新涉及**中断性变更**。</span><span class="sxs-lookup"><span data-stu-id="6c373-110">This update involves **breaking changes**.</span></span>

<span data-ttu-id="6c373-111">请阅读[从 v0.21.1 迁移到 v0.50.0](#migration-from-v0211-to-v0500)，确定目前是否适合升级。</span><span class="sxs-lookup"><span data-stu-id="6c373-111">Please read [Migration from v0.21.1 to v0.50.0](#migration-from-v0211-to-v0500) to determine if upgrading is right for you at this time.</span></span>

<span data-ttu-id="6c373-112">新的基于 AMQP 的 API 提供改进的消息传递可靠性、性能以及将来的扩展功能支持。</span><span class="sxs-lookup"><span data-stu-id="6c373-112">The new AMQP-based API offers improved message passing reliability, performance and expanded feature support going forward.</span></span>
<span data-ttu-id="6c373-113">新 API 还提供基于 asyncio 的异步操作支持，可以通过异步操作发送、接收和处理消息。</span><span class="sxs-lookup"><span data-stu-id="6c373-113">The new API also offers support for asynchronous operations (based on asyncio) for sending, receiving and handling messages.</span></span>

<span data-ttu-id="6c373-114">有关基于 HTTP 的旧操作的文档，请参阅[使用旧 API 的基于 HTTP 的操作](#using-http-based-operations-of-the-legacy-api)。</span><span class="sxs-lookup"><span data-stu-id="6c373-114">For documentation on the legacy HTTP-based operations please see [Using HTTP-based operations of the legacy API](#using-http-based-operations-of-the-legacy-api).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c373-115">先决条件</span><span class="sxs-lookup"><span data-stu-id="6c373-115">Prerequisites</span></span>

* <span data-ttu-id="6c373-116">Azure 订阅 - [创建免费帐户](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="6c373-116">Azure subscription - [Create a free account](https://azure.microsoft.com/free/)</span></span>
* <span data-ttu-id="6c373-117">Azure [服务总线命名空间和管理凭据](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span><span class="sxs-lookup"><span data-stu-id="6c373-117">Azure [Service Bus namespace and management credentials](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span></span>
* [<span data-ttu-id="6c373-118">Python 2.7-3.7</span><span class="sxs-lookup"><span data-stu-id="6c373-118">Python 2.7-3.7</span></span>](https://www.python.org/downloads/)

## <a name="installation"></a><span data-ttu-id="6c373-119">安装</span><span class="sxs-lookup"><span data-stu-id="6c373-119">Installation</span></span>

```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a><span data-ttu-id="6c373-120">连接到 Azure 服务总线</span><span class="sxs-lookup"><span data-stu-id="6c373-120">Connect to Azure Service Bus</span></span>

### <a name="get-credentials"></a><span data-ttu-id="6c373-121">获取凭据</span><span class="sxs-lookup"><span data-stu-id="6c373-121">Get credentials</span></span>

<span data-ttu-id="6c373-122">使用下面的 Azure CLI 代码片段为环境变量填充服务总线连接字符串（也可在 Azure 门户中找到此值）。</span><span class="sxs-lookup"><span data-stu-id="6c373-122">Use the Azure CLI snippet below to populate an environment variable with the Service Bus connection string (you can also find this value in the Azure portal).</span></span> <span data-ttu-id="6c373-123">此代码片段已针对 Bash shell 格式化。</span><span class="sxs-lookup"><span data-stu-id="6c373-123">The snippet is formatted for the Bash shell.</span></span>

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

### <a name="create-client"></a><span data-ttu-id="6c373-124">创建客户端</span><span class="sxs-lookup"><span data-stu-id="6c373-124">Create client</span></span>

<span data-ttu-id="6c373-125">填充 `SB_CONN_STR` 环境变量后，即可创建 ServiceBusClient。</span><span class="sxs-lookup"><span data-stu-id="6c373-125">Once you've populated the `SB_CONN_STR` environment variable, you can create the ServiceBusClient.</span></span>

```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

<span data-ttu-id="6c373-126">如果希望使用异步操作，请使用 `azure.servicebus.aio` 命名空间。</span><span class="sxs-lookup"><span data-stu-id="6c373-126">If you wish to use asynchronous operations, please use the `azure.servicebus.aio` namespace.</span></span>

```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

## <a name="service-bus-queues"></a><span data-ttu-id="6c373-127">服务总线队列</span><span class="sxs-lookup"><span data-stu-id="6c373-127">Service Bus queues</span></span>

<span data-ttu-id="6c373-128">服务总线队列是存储队列的替代方法，可能在需要使用推送型传递（使用长轮询）执行更高级的消息功能（较大的消息大小、消息排序、单操作破坏性读取、按计划执行的传送）时十分有用。</span><span class="sxs-lookup"><span data-stu-id="6c373-128">Service Bus queues are an alternative to Storage queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

### <a name="create-queue"></a><span data-ttu-id="6c373-129">创建队列</span><span class="sxs-lookup"><span data-stu-id="6c373-129">Create queue</span></span>

<span data-ttu-id="6c373-130">这样会在服务总线命名空间中创建新的队列。</span><span class="sxs-lookup"><span data-stu-id="6c373-130">This creates a new queue within the Service Bus namespace.</span></span> <span data-ttu-id="6c373-131">如果命名空间中已经存在相同名称的队列，则会引发错误。</span><span class="sxs-lookup"><span data-stu-id="6c373-131">If a queue of the same name already exists within the namespace an error will be raised.</span></span>

```python
sb_client.create_queue("MyQueue")
```

<span data-ttu-id="6c373-132">也可指定用于配置队列行为的可选参数。</span><span class="sxs-lookup"><span data-stu-id="6c373-132">Optional parameters to configure the queue behavior can also be specified.</span></span>

```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a><span data-ttu-id="6c373-133">获取队列客户端</span><span class="sxs-lookup"><span data-stu-id="6c373-133">Get a queue client</span></span>

<span data-ttu-id="6c373-134">可以使用 QueueClient 通过队列发送和接收消息，以及执行其他操作。</span><span class="sxs-lookup"><span data-stu-id="6c373-134">A QueueClient can be used to send and receive messages from the queue, along with other operations.</span></span>

```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a><span data-ttu-id="6c373-135">发送消息</span><span class="sxs-lookup"><span data-stu-id="6c373-135">Sending messages</span></span>

<span data-ttu-id="6c373-136">此队列客户端可以一次发送一条或多条消息：</span><span class="sxs-lookup"><span data-stu-id="6c373-136">The queue client can send one or more messages at a time:</span></span>

```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```

<span data-ttu-id="6c373-137">每次调用 QueueClient.send 都会创建新的服务连接。</span><span class="sxs-lookup"><span data-stu-id="6c373-137">Each call to QueueClient.send will create a new service connection.</span></span> <span data-ttu-id="6c373-138">若要将同一连接重复用于多个发送调用，可以打开一个发送程序：</span><span class="sxs-lookup"><span data-stu-id="6c373-138">To reuse the same connection for multiple send calls, you can open a sender:</span></span>

```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```

<span data-ttu-id="6c373-139">如果使用异步客户端，则上述操作会使用异步语法：</span><span class="sxs-lookup"><span data-stu-id="6c373-139">If you are using an asynchronous client, the above operations will use async syntax:</span></span>

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

### <a name="receiving-messages"></a><span data-ttu-id="6c373-140">接收消息</span><span class="sxs-lookup"><span data-stu-id="6c373-140">Receiving messages</span></span>

<span data-ttu-id="6c373-141">可以从队列以连续迭代器的方式接收消息。</span><span class="sxs-lookup"><span data-stu-id="6c373-141">Messages can be received from a queue as a continuous iterator.</span></span> <span data-ttu-id="6c373-142">进行消息接收的默认模式是 [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read)，此模式要求显式完成每条消息，以便将其从队列中删除。</span><span class="sxs-lookup"><span data-stu-id="6c373-142">The default mode for message receiving is [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), which requires each message to be explicitly completed in order that it be removed from the queue.</span></span>

```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```

<span data-ttu-id="6c373-143">在迭代器运行期间，服务连接将始终保持打开状态。</span><span class="sxs-lookup"><span data-stu-id="6c373-143">The service connection will remain open for the entirety of the iterator.</span></span> <span data-ttu-id="6c373-144">如果发现自己只对消息流进行了部分迭代，则应在 `with` 语句中运行接收器，确保连接关闭：</span><span class="sxs-lookup"><span data-stu-id="6c373-144">If you find yourself only partially iterating the message stream, you should run the receiver in a `with` statement to ensure the connection is closed:</span></span>

```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
<span data-ttu-id="6c373-145">如果使用异步客户端，则上述操作会使用异步语法：</span><span class="sxs-lookup"><span data-stu-id="6c373-145">If you are using an asynchronous client, the above operations will use async syntax:</span></span>

```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a><span data-ttu-id="6c373-146">服务总线主题和订阅</span><span class="sxs-lookup"><span data-stu-id="6c373-146">Service Bus topics and subscriptions</span></span>

<span data-ttu-id="6c373-147">服务总线主题和订阅是基于服务总线队列的抽象，该队列以“发布/订阅”模式提供一对多的通信形式。</span><span class="sxs-lookup"><span data-stu-id="6c373-147">Service Bus topics and subscriptions are an abstraction on top of Service Bus queues that provide a one-to-many form of communication, in a publish/subscribe pattern.</span></span> <span data-ttu-id="6c373-148">消息发送到一个主题并传送到一个或多个关联订阅，这可以用来扩大接收方的数目。</span><span class="sxs-lookup"><span data-stu-id="6c373-148">Messages are sent to a topic and delivered to one or more associated subscriptions, which is useful for scaling to large numbers of recipients.</span></span>

### <a name="create-topic"></a><span data-ttu-id="6c373-149">创建主题</span><span class="sxs-lookup"><span data-stu-id="6c373-149">Create topic</span></span>

<span data-ttu-id="6c373-150">这样会在服务总线命名空间中创建新的主题。</span><span class="sxs-lookup"><span data-stu-id="6c373-150">This creates a new topic within the Service Bus namespace.</span></span> <span data-ttu-id="6c373-151">如果已经存在相同名称的主题，则会引发错误。</span><span class="sxs-lookup"><span data-stu-id="6c373-151">If a topic of the same name already exists an error will be raised.</span></span>

```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a><span data-ttu-id="6c373-152">获取主题客户端</span><span class="sxs-lookup"><span data-stu-id="6c373-152">Get a topic client</span></span>

<span data-ttu-id="6c373-153">可以使用 TopicClient 将消息发送到主题，以及执行其他操作。</span><span class="sxs-lookup"><span data-stu-id="6c373-153">A TopicClient can be used to send messages to the topic, along with other operations.</span></span>

```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a><span data-ttu-id="6c373-154">创建订阅</span><span class="sxs-lookup"><span data-stu-id="6c373-154">Create subscription</span></span>

<span data-ttu-id="6c373-155">这样会在服务总线命名空间中为指定主题创建新的订阅。</span><span class="sxs-lookup"><span data-stu-id="6c373-155">This creates a new subscription for the specified topic within the Service Bus namespace.</span></span>

```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a><span data-ttu-id="6c373-156">获取订阅客户端</span><span class="sxs-lookup"><span data-stu-id="6c373-156">Get a subscription client</span></span>

<span data-ttu-id="6c373-157">可以使用 SubscriptionClient 通过主题接收消息，以及执行其他操作。</span><span class="sxs-lookup"><span data-stu-id="6c373-157">A SubscriptionClient can be used to receive messages from the topic, along with other operations.</span></span>

```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a><span data-ttu-id="6c373-158">从 v0.21.1 迁移到 v0.50.0</span><span class="sxs-lookup"><span data-stu-id="6c373-158">Migration from v0.21.1 to v0.50.0</span></span>

<span data-ttu-id="6c373-159">版本 0.50.0 中引入了主要的中断性变更。</span><span class="sxs-lookup"><span data-stu-id="6c373-159">Major breaking changes were introduced in version 0.50.0.</span></span> <span data-ttu-id="6c373-160">原始的基于 HTTP 的 API 在 v0.50.0 中仍可用 - 不过，它现在存在于新的命名空间：`azure.servicebus.control_client`。</span><span class="sxs-lookup"><span data-stu-id="6c373-160">The original HTTP-based API is still available in v0.50.0 - however it now exists under a new namesapce: `azure.servicebus.control_client`.</span></span>

### <a name="should-i-upgrade"></a><span data-ttu-id="6c373-161">是否应该升级？</span><span class="sxs-lookup"><span data-stu-id="6c373-161">Should I upgrade?</span></span>

<span data-ttu-id="6c373-162">与 v0.21.1 相比，新包 (v0.50.0) 没有在基于 HTTP 的操作方面提供任何改进。</span><span class="sxs-lookup"><span data-stu-id="6c373-162">The new package (v0.50.0) offers no improvements in HTTP-based operations over v0.21.1.</span></span> <span data-ttu-id="6c373-163">基于 HTTP 的 API 未做更改，只是现在存在于新的命名空间。</span><span class="sxs-lookup"><span data-stu-id="6c373-163">The HTTP-based API is identical except that it now exists under a new namespace.</span></span> <span data-ttu-id="6c373-164">因此，如果只是希望使用基于 HTTP 的操作（`create_queue`、`delete_queue` 等），则现在升级没有额外的好处。</span><span class="sxs-lookup"><span data-stu-id="6c373-164">For this reason if you only wish to use HTTP-based operations (`create_queue`, `delete_queue` etc) - there will be no additional benefit in upgrading at this time.</span></span>

### <a name="how-do-i-migrate-my-code-to-the-new-version"></a><span data-ttu-id="6c373-165">如何将代码迁移到新版本？</span><span class="sxs-lookup"><span data-stu-id="6c373-165">How do I migrate my code to the new version?</span></span>

<span data-ttu-id="6c373-166">针对 v0.21.0 编写的代码可以移植到版本 0.50.0，只需更改导入命名空间即可：</span><span class="sxs-lookup"><span data-stu-id="6c373-166">Code written against v0.21.0 can be ported to version 0.50.0 by simply changing the import namespace:</span></span>

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a><span data-ttu-id="6c373-167">使用旧 API 的基于 HTTP 的操作</span><span class="sxs-lookup"><span data-stu-id="6c373-167">Using HTTP-based operations of the legacy API</span></span>

<span data-ttu-id="6c373-168">以下文档介绍旧版 API，应该适用于那些希望将现有代码移植到 v0.50.0 但不做任何其他更改的用户。</span><span class="sxs-lookup"><span data-stu-id="6c373-168">The following documentation describes the legacy API and should be used for those wishing to port existing code to v0.50.0 without making any additional changes.</span></span> <span data-ttu-id="6c373-169">此参考也可用作那些使用 v0.21.1 的用户的指南。</span><span class="sxs-lookup"><span data-stu-id="6c373-169">This reference can also be used as guidance by those using v0.21.1.</span></span>
<span data-ttu-id="6c373-170">对于那些编写新代码的用户，建议使用上面介绍的新 API。</span><span class="sxs-lookup"><span data-stu-id="6c373-170">For those writing new code, we recommend using the new API described above.</span></span>

### <a name="service-bus-queues"></a><span data-ttu-id="6c373-171">服务总线队列</span><span class="sxs-lookup"><span data-stu-id="6c373-171">Service Bus queues</span></span>

#### <a name="shared-access-signature-sas-authentication"></a><span data-ttu-id="6c373-172">共享访问签名 (SAS) 身份验证</span><span class="sxs-lookup"><span data-stu-id="6c373-172">Shared Access Signature (SAS) authentication</span></span>

<span data-ttu-id="6c373-173">若要使用共享访问签名身份验证，请使用以下代码创建服务总线服务：</span><span class="sxs-lookup"><span data-stu-id="6c373-173">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a><span data-ttu-id="6c373-174">访问控制服务 (ACS) 身份验证</span><span class="sxs-lookup"><span data-stu-id="6c373-174">Access Control Service (ACS) authentication</span></span>

<span data-ttu-id="6c373-175">新的服务总线命名空间不支持 ACS。</span><span class="sxs-lookup"><span data-stu-id="6c373-175">ACS is not supported on new Service Bus namespaces.</span></span> <span data-ttu-id="6c373-176">建议[将应用程序迁移到 SAS 身份验证](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)。</span><span class="sxs-lookup"><span data-stu-id="6c373-176">We recommend [migrating applications to SAS authentication](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span></span> <span data-ttu-id="6c373-177">若要在旧版服务总线命名空间中使用 ACS 身份验证，请使用以下命令创建 ServiceBusService：</span><span class="sxs-lookup"><span data-stu-id="6c373-177">To use ACS authentication within an older Service Bus namesapce, create the ServiceBusService with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```

#### <a name="sending-and-receiving-messages"></a><span data-ttu-id="6c373-178">发送和接收消息</span><span class="sxs-lookup"><span data-stu-id="6c373-178">Sending and receiving messages</span></span>

<span data-ttu-id="6c373-179">**create\_queue** 方法可用于确保队列存在：</span><span class="sxs-lookup"><span data-stu-id="6c373-179">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```

<span data-ttu-id="6c373-180">然后可以调用 **send\_queue\_message** 方法将消息插入到队列：</span><span class="sxs-lookup"><span data-stu-id="6c373-180">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="6c373-181">然后可以调用 **send\_queue\_message_batch** 方法来一次发送多条消息：</span><span class="sxs-lookup"><span data-stu-id="6c373-181">The **send\_queue\_message_batch** method can then be called to  send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```

<span data-ttu-id="6c373-182">然后可以调用 **receive\_queue\_message** 方法将消息取消排队。</span><span class="sxs-lookup"><span data-stu-id="6c373-182">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a><span data-ttu-id="6c373-183">服务总线主题</span><span class="sxs-lookup"><span data-stu-id="6c373-183">Service Bus topics</span></span>

<span data-ttu-id="6c373-184">**create\_topic** 方法可用于创建服务器端主题：</span><span class="sxs-lookup"><span data-stu-id="6c373-184">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```

<span data-ttu-id="6c373-185">**send\_topic\_message** 方法可用于向主题发送一个消息：</span><span class="sxs-lookup"><span data-stu-id="6c373-185">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="6c373-186">**send\_topic\_message_batch** 方法可用于一次发送多条消息：</span><span class="sxs-lookup"><span data-stu-id="6c373-186">The **send\_topic\_message_batch** method can be used to send  several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="6c373-187">请考虑，在 Python 3 中，字符串消息将进行 utf-8 编码，并且你必须在 Python 2 中自行管理你的编码。</span><span class="sxs-lookup"><span data-stu-id="6c373-187">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="6c373-188">然后，客户端可以创建订阅并开始通过调用 **create\_subscription** 方法（后跟 **receive\_subscription\_message** 方法）使用消息。</span><span class="sxs-lookup"><span data-stu-id="6c373-188">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="6c373-189">请注意，创建订阅之前发送的任何消息均不会被收到。</span><span class="sxs-lookup"><span data-stu-id="6c373-189">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a><span data-ttu-id="6c373-190">事件中心</span><span class="sxs-lookup"><span data-stu-id="6c373-190">Event Hub</span></span>

<span data-ttu-id="6c373-191">使用事件中心能够以高呑吐量从一组不同的设备和服务收集事件流。</span><span class="sxs-lookup"><span data-stu-id="6c373-191">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="6c373-192">**create\_event\_hub** 方法可用于创建事件中心：</span><span class="sxs-lookup"><span data-stu-id="6c373-192">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```

<span data-ttu-id="6c373-193">若要发送事件，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="6c373-193">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```

<span data-ttu-id="6c373-194">事件内容是事件消息或包含多个消息的 JSON 编码字符串。</span><span class="sxs-lookup"><span data-stu-id="6c373-194">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

### <a name="advanced-features"></a><span data-ttu-id="6c373-195">高级功能</span><span class="sxs-lookup"><span data-stu-id="6c373-195">Advanced features</span></span>

#### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="6c373-196">Broker 属性和 User 属性</span><span class="sxs-lookup"><span data-stu-id="6c373-196">Broker properties and user properties</span></span>

<span data-ttu-id="6c373-197">本部分介绍如何使用[此处](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)定义的 Broker 和 User 属性：</span><span class="sxs-lookup"><span data-stu-id="6c373-197">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```

<span data-ttu-id="6c373-198">可以使用 datetime、int、float 或 boolean</span><span class="sxs-lookup"><span data-stu-id="6c373-198">You can use datetime, int, float or boolean</span></span>

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

<span data-ttu-id="6c373-199">出于与此库的旧版本的兼容性原因，`broker_properties` 也可以定义为 JSON 字符串。</span><span class="sxs-lookup"><span data-stu-id="6c373-199">For compatibility reason with old version of this library,  `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="6c373-200">如果是这种情况，那么你要负责编写有效的 JSON 字符串，因为在发送到 RestAPI 之前，Python 不会执行任何检查。</span><span class="sxs-lookup"><span data-stu-id="6c373-200">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a><span data-ttu-id="6c373-201">后续步骤</span><span class="sxs-lookup"><span data-stu-id="6c373-201">Next Steps</span></span>

* [<span data-ttu-id="6c373-202">服务总线文档</span><span class="sxs-lookup"><span data-stu-id="6c373-202">Service Bus documentation</span></span>](https://docs.microsoft.com/azure/service-bus-messaging)
* [<span data-ttu-id="6c373-203">SDK 源代码</span><span class="sxs-lookup"><span data-stu-id="6c373-203">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="6c373-204">SDK 参考文档</span><span class="sxs-lookup"><span data-stu-id="6c373-204">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [<span data-ttu-id="6c373-205">其他示例</span><span class="sxs-lookup"><span data-stu-id="6c373-205">Additional samples</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
