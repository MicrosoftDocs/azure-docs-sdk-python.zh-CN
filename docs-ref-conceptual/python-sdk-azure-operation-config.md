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
# <a name="operation-config"></a><span data-ttu-id="c38eb-104">操作配置</span><span class="sxs-lookup"><span data-stu-id="c38eb-104">Operation config</span></span> 

<span data-ttu-id="c38eb-105">操作上的方法具有可以在 `kwargs` 中提供的额外参数。</span><span class="sxs-lookup"><span data-stu-id="c38eb-105">Methods on operations have extra parameters which can be provided in the `kwargs`.</span></span> <span data-ttu-id="c38eb-106">这称为 operation_config。</span><span class="sxs-lookup"><span data-stu-id="c38eb-106">This is called operation_config.</span></span>

<span data-ttu-id="c38eb-107">操作配置选项包括：</span><span class="sxs-lookup"><span data-stu-id="c38eb-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="c38eb-108">参数名称</span><span class="sxs-lookup"><span data-stu-id="c38eb-108">Parameter name</span></span>|<span data-ttu-id="c38eb-109">Type</span><span class="sxs-lookup"><span data-stu-id="c38eb-109">Type</span></span>|<span data-ttu-id="c38eb-110">角色</span><span class="sxs-lookup"><span data-stu-id="c38eb-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="c38eb-111">验证</span><span class="sxs-lookup"><span data-stu-id="c38eb-111">verify</span></span> |`bool`|<span data-ttu-id="c38eb-112">是否要验证 SSL 证书。</span><span class="sxs-lookup"><span data-stu-id="c38eb-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="c38eb-113">默认值为 True。</span><span class="sxs-lookup"><span data-stu-id="c38eb-113">Default is True.</span></span>|
|  <span data-ttu-id="c38eb-114">cert</span><span class="sxs-lookup"><span data-stu-id="c38eb-114">cert</span></span> |`str`| <span data-ttu-id="c38eb-115">用于客户端端验证的本地证书的路径。</span><span class="sxs-lookup"><span data-stu-id="c38eb-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="c38eb-116">timeout</span><span class="sxs-lookup"><span data-stu-id="c38eb-116">timeout</span></span> |`int`| <span data-ttu-id="c38eb-117">用于建立服务器连接的超时值（以秒为单位）。</span><span class="sxs-lookup"><span data-stu-id="c38eb-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="c38eb-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="c38eb-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="c38eb-119">是否允许重定向。</span><span class="sxs-lookup"><span data-stu-id="c38eb-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="c38eb-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="c38eb-120">max_redirects</span></span>  |`int`| <span data-ttu-id="c38eb-121">允许的最大重定向数目。</span><span class="sxs-lookup"><span data-stu-id="c38eb-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="c38eb-122">proxies</span><span class="sxs-lookup"><span data-stu-id="c38eb-122">proxies</span></span>  |`dict` |<span data-ttu-id="c38eb-123">代理服务器设置。</span><span class="sxs-lookup"><span data-stu-id="c38eb-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="c38eb-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="c38eb-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="c38eb-125">是否要从本地环境变量读取代理设置。</span><span class="sxs-lookup"><span data-stu-id="c38eb-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="c38eb-126">retries</span><span class="sxs-lookup"><span data-stu-id="c38eb-126">retries</span></span>  |`int` | <span data-ttu-id="c38eb-127">尝试重试的总次数。</span><span class="sxs-lookup"><span data-stu-id="c38eb-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="c38eb-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="c38eb-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="c38eb-129">以调试模式启用 HTTP 日志（默认情况下为 False）。</span><span class="sxs-lookup"><span data-stu-id="c38eb-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
