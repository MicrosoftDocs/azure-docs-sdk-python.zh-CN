---
title: "安装"
description: "如何安装 Azure Python SDK"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5ce4ef27667d45697200eef67be92c62812b3809
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2017
---
# <a name="installation"></a><span data-ttu-id="94689-104">安装</span><span class="sxs-lookup"><span data-stu-id="94689-104">Installation</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="94689-105">使用 pip 安装</span><span class="sxs-lookup"><span data-stu-id="94689-105">Installation with pip</span></span>

<span data-ttu-id="94689-106">可以单独安装每个 Azure 服务的库：</span><span class="sxs-lookup"><span data-stu-id="94689-106">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="94689-107">可以使用 `--pre` 标志安装预览包：</span><span class="sxs-lookup"><span data-stu-id="94689-107">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="94689-108">还可以使用 `azure` 元程序包在单个行中安装一组 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="94689-108">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="94689-109">我们发布了此包的预览版，可以使用 --pre 标志进行访问：</span><span class="sxs-lookup"><span data-stu-id="94689-109">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="94689-110">从 GitHub 安装</span><span class="sxs-lookup"><span data-stu-id="94689-110">Install from GitHub</span></span>

<span data-ttu-id="94689-111">如果想要从源安装 `azure`：</span><span class="sxs-lookup"><span data-stu-id="94689-111">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
