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
ms.openlocfilehash: d7be7cd76d019c6741d93c04458376a9352e363b
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
ms.locfileid: "30204256"
---
# <a name="operation-config"></a><span data-ttu-id="b93da-104">操作配置</span><span class="sxs-lookup"><span data-stu-id="b93da-104">Operation config</span></span> 

<span data-ttu-id="b93da-105">操作上的方法具有可以在 kwargs 中提供的额外参数。</span><span class="sxs-lookup"><span data-stu-id="b93da-105">Methods on operations have extra parameters which can be provided in the kwargs.</span></span> <span data-ttu-id="b93da-106">这称为 operation_config。</span><span class="sxs-lookup"><span data-stu-id="b93da-106">This is called operation_config.</span></span>

<span data-ttu-id="b93da-107">操作配置选项包括：</span><span class="sxs-lookup"><span data-stu-id="b93da-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="b93da-108">参数名称</span><span class="sxs-lookup"><span data-stu-id="b93da-108">Parameter name</span></span>|<span data-ttu-id="b93da-109">类型</span><span class="sxs-lookup"><span data-stu-id="b93da-109">Type</span></span>|<span data-ttu-id="b93da-110">角色</span><span class="sxs-lookup"><span data-stu-id="b93da-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="b93da-111">verify</span><span class="sxs-lookup"><span data-stu-id="b93da-111">verify</span></span> |`bool`|<span data-ttu-id="b93da-112">是否要验证 SSL 证书。</span><span class="sxs-lookup"><span data-stu-id="b93da-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="b93da-113">默认值为 True。</span><span class="sxs-lookup"><span data-stu-id="b93da-113">Default is True.</span></span>|
|  <span data-ttu-id="b93da-114">cert</span><span class="sxs-lookup"><span data-stu-id="b93da-114">cert</span></span> |`str`| <span data-ttu-id="b93da-115">用于客户端端验证的本地证书的路径。</span><span class="sxs-lookup"><span data-stu-id="b93da-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="b93da-116">timeout</span><span class="sxs-lookup"><span data-stu-id="b93da-116">timeout</span></span> |`int`| <span data-ttu-id="b93da-117">用于建立服务器连接的超时值（以秒为单位）。</span><span class="sxs-lookup"><span data-stu-id="b93da-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="b93da-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="b93da-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="b93da-119">是否允许重定向。</span><span class="sxs-lookup"><span data-stu-id="b93da-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="b93da-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="b93da-120">max_redirects</span></span>  |`int`| <span data-ttu-id="b93da-121">允许的最大重定向数目。</span><span class="sxs-lookup"><span data-stu-id="b93da-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="b93da-122">proxies</span><span class="sxs-lookup"><span data-stu-id="b93da-122">proxies</span></span>  |`dict` |<span data-ttu-id="b93da-123">代理服务器设置。</span><span class="sxs-lookup"><span data-stu-id="b93da-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="b93da-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="b93da-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="b93da-125">是否要从本地环境变量读取代理设置。</span><span class="sxs-lookup"><span data-stu-id="b93da-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="b93da-126">retries</span><span class="sxs-lookup"><span data-stu-id="b93da-126">retries</span></span>  |`int` | <span data-ttu-id="b93da-127">尝试重试的总次数。</span><span class="sxs-lookup"><span data-stu-id="b93da-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="b93da-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="b93da-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="b93da-129">以调试模式启用 HTTP 日志（默认情况下为 False）。</span><span class="sxs-lookup"><span data-stu-id="b93da-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
