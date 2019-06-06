---
title: 用于 Python 操作配置的 Azure SDK
description: 用于 Python 的 Azure SDK 引发的 C
keywords: Azure, Python, SDK, API, 异常
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 03/07/2018
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: adeb6aa8a2c363c3119e97503df9536fb0633b4c
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376867"
---
# <a name="operation-config"></a>操作配置 

操作上的方法具有可以在 `kwargs` 中提供的额外参数。 这称为 operation_config。

操作配置选项包括：

|参数名称|Type|角色|
|----------------------|------|---------------|
| 验证 |`bool`|是否要验证 SSL 证书。 默认值为 True。|
|  cert |`str`| 用于客户端端验证的本地证书的路径。|
|  timeout |`int`| 用于建立服务器连接的超时值（以秒为单位）。|
|  allow_redirects |`bool` | 是否允许重定向。|
|  max_redirects  |`int`| 允许的最大重定向数目。|
|  proxies  |`dict` |代理服务器设置。|
|  use_env_proxies |`bool` |是否要从本地环境变量读取代理设置。|
|  retries  |`int` | 尝试重试的总次数。|
|  enable_http_logger | `bool`| 以调试模式启用 HTTP 日志（默认情况下为 False）。|
