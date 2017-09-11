---
title: "用于 Python 的服务总线库"
description: "用于服务总线的 Python 客户端和管理库的参考文档"
keywords: "Azure, Python, SDK, API, 消息传送, pubsub, 发布-订阅, 消息中转站"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: bf7be945f2c7a3daea93ff4e5b770459c00632c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="78e06-104">用于 Python 的服务总线库</span><span class="sxs-lookup"><span data-stu-id="78e06-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="78e06-105">概述</span><span class="sxs-lookup"><span data-stu-id="78e06-105">Overview</span></span>

<span data-ttu-id="78e06-106">Microsoft Azure 服务总线支持一组基于云的、面向消息的中间件技术，包括可靠的消息队列和持久的发布/订阅消息。</span><span class="sxs-lookup"><span data-stu-id="78e06-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="78e06-107">安装库</span><span class="sxs-lookup"><span data-stu-id="78e06-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a><span data-ttu-id="78e06-108">示例</span><span class="sxs-lookup"><span data-stu-id="78e06-108">Example</span></span>
<span data-ttu-id="78e06-109">向队列发送消息。</span><span class="sxs-lookup"><span data-stu-id="78e06-109">Send messages to a queue.</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="78e06-110">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="78e06-110">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/managementlibrary)

