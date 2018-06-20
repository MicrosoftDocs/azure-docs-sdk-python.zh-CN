---
title: 安装
description: 如何安装 Azure Python SDK
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
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
ms.locfileid: "29565816"
---
# <a name="installation"></a><span data-ttu-id="410b6-104">安装</span><span class="sxs-lookup"><span data-stu-id="410b6-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="410b6-105">要使用哪种 Python 以及哪个版本</span><span class="sxs-lookup"><span data-stu-id="410b6-105">Which Python and which version to use</span></span>
<span data-ttu-id="410b6-106">有多个 Python 解释程序可用 - 示例包括：</span><span class="sxs-lookup"><span data-stu-id="410b6-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="410b6-107">CPython - 最常用的标准 Python 解释程序</span><span class="sxs-lookup"><span data-stu-id="410b6-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="410b6-108">PyPy - 快速、CPython 的合规替代实现</span><span class="sxs-lookup"><span data-stu-id="410b6-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="410b6-109">IronPython - 在 .Net/CLR 上运行的 Python 解释程序</span><span class="sxs-lookup"><span data-stu-id="410b6-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="410b6-110">Jython - 在 Java 虚拟机上运行的 Python 解释程序</span><span class="sxs-lookup"><span data-stu-id="410b6-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="410b6-111">**CPython** v2.7 或 v3.4+ 和 PyPy 5.4.0 已经过测试，并且受 Python Azure SDK 支持。</span><span class="sxs-lookup"><span data-stu-id="410b6-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="410b6-112">从哪里获得 Python？</span><span class="sxs-lookup"><span data-stu-id="410b6-112">Where to get Python?</span></span>
<span data-ttu-id="410b6-113">有多种方法可获得 CPython：</span><span class="sxs-lookup"><span data-stu-id="410b6-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="410b6-114">直接从 [Python](https://www.python.org/) 获得</span><span class="sxs-lookup"><span data-stu-id="410b6-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="410b6-115">从可信发行版（如[Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/) 或 [ActiveState](https://www.activestate.com/)）获得</span><span class="sxs-lookup"><span data-stu-id="410b6-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="410b6-116">从源构建！</span><span class="sxs-lookup"><span data-stu-id="410b6-116">Build from source!</span></span>

<span data-ttu-id="410b6-117">除非有特定需求，否则建议使用前两个选项。</span><span class="sxs-lookup"><span data-stu-id="410b6-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="410b6-118">使用 pip 安装</span><span class="sxs-lookup"><span data-stu-id="410b6-118">Installation with pip</span></span>

<span data-ttu-id="410b6-119">可以单独安装每个 Azure 服务的库：</span><span class="sxs-lookup"><span data-stu-id="410b6-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="410b6-120">可以使用 `--pre` 标志安装预览包：</span><span class="sxs-lookup"><span data-stu-id="410b6-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="410b6-121">还可以使用 `azure` 元程序包在单个行中安装一组 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="410b6-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="410b6-122">我们发布了此包的预览版，可以使用 --pre 标志进行访问：</span><span class="sxs-lookup"><span data-stu-id="410b6-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="410b6-123">从 GitHub 安装</span><span class="sxs-lookup"><span data-stu-id="410b6-123">Install from GitHub</span></span>

<span data-ttu-id="410b6-124">如果想要从源安装 `azure`：</span><span class="sxs-lookup"><span data-stu-id="410b6-124">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install