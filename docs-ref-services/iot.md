---
title: "用于 Python 的 Azure IoT 中心库"
description: "用于 Python 的 Azure IoT 中心库参考"
keywords: "Azure, python, SDK, API, IoT 中心"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 0a1a5efa299f66ff8c31e8224e29dd7bcdc41783
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="azure-iot-hub-libraries-for-python"></a><span data-ttu-id="a8ffb-104">用于 Python 的 Azure IoT 中心库</span><span class="sxs-lookup"><span data-stu-id="a8ffb-104">Azure IoT Hub libraries for python</span></span>

## <a name="management-apipythonapioverviewazureiotmanagement"></a>[<span data-ttu-id="a8ffb-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="a8ffb-105">Management API</span></span>](/python/api/overview/azure/iot/management)

```bash
pip install azure-mgmt-iothub
```

## <a name="create-the-management-client"></a><span data-ttu-id="a8ffb-106">创建管理客户端</span><span class="sxs-lookup"><span data-stu-id="a8ffb-106">Create the management client</span></span>

<span data-ttu-id="a8ffb-107">以下代码创建管理客户端的实例。</span><span class="sxs-lookup"><span data-stu-id="a8ffb-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="a8ffb-108">你需要提供可以从[订阅列表](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)检索的 ``subscription_id``。</span><span class="sxs-lookup"><span data-stu-id="a8ffb-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="a8ffb-109">有关使用 Python SDK 处理 Azure Active Directory 身份验证以及创建 ``Credentials`` 实例的详细信息，请参阅[资源管理身份验证](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="a8ffb-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.iothub import IotHubClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

iothub_client = IotHubClient(
    credentials,
    subscription_id
)
```

## <a name="create-an-iothub"></a><span data-ttu-id="a8ffb-110">创建 IoT 中心</span><span class="sxs-lookup"><span data-stu-id="a8ffb-110">Create an IoTHub</span></span>
```python
async_iot_hub = iothub_client.iot_hub_resource.create_or_update(
    'MyResourceGroup',
    'MyIoTHubAccount',
    {
        'location': 'westus',
        'subscriptionid': subscription_id,
        'resourcegroup': 'MyResourceGroup',
        'sku': {
            'name': 'S1',
            'capacity': 2
        },
        'properties': {
            'enable_file_upload_notifications': False,
            'operations_monitoring_properties': {
            'events': {
                "C2DCommands": "Error",
                "DeviceTelemetry": "Error",
                "DeviceIdentityOperations": "Error",
                "Connections": "Information"
            }
            },
            "features": "None",
        }
    }
)
iothub = async_iot_hub.result() # Blocking wait for creation
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="a8ffb-111">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="a8ffb-111">Explore the Management APIs</span></span>](/python/api/overview/azure/iot/management)