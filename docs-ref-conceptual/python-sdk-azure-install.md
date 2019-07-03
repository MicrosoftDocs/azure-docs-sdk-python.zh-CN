---
title: 安装
description: 如何安装 Azure Python SDK
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 478118642122d7c0c80b1ddf37b13f71d8ca3adc
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534450"
---
# <a name="installation"></a><span data-ttu-id="b007a-104">安装</span><span class="sxs-lookup"><span data-stu-id="b007a-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="b007a-105">要使用哪种 Python 以及哪个版本</span><span class="sxs-lookup"><span data-stu-id="b007a-105">Which Python and which version to use</span></span>

<span data-ttu-id="b007a-106">有多个 Python 解释程序可用 - 示例包括：</span><span class="sxs-lookup"><span data-stu-id="b007a-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="b007a-107">CPython - 最常用的标准 Python 解释程序</span><span class="sxs-lookup"><span data-stu-id="b007a-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="b007a-108">PyPy - 快速、CPython 的合规替代实现</span><span class="sxs-lookup"><span data-stu-id="b007a-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="b007a-109">IronPython - 在 .Net/CLR 上运行的 Python 解释程序</span><span class="sxs-lookup"><span data-stu-id="b007a-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="b007a-110">Jython - 在 Java 虚拟机上运行的 Python 解释程序</span><span class="sxs-lookup"><span data-stu-id="b007a-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="b007a-111">**CPython** v2.7 或 v3.4+ 和 PyPy 5.4.0 已经过测试，并且受 Python Azure SDK 支持。</span><span class="sxs-lookup"><span data-stu-id="b007a-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="b007a-112">从哪里获得 Python？</span><span class="sxs-lookup"><span data-stu-id="b007a-112">Where to get Python?</span></span>

<span data-ttu-id="b007a-113">有多种方法可获得 CPython：</span><span class="sxs-lookup"><span data-stu-id="b007a-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="b007a-114">直接从 [Python](https://www.python.org/) 获得</span><span class="sxs-lookup"><span data-stu-id="b007a-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="b007a-115">从可信发行版（如[Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/) 或 [ActiveState](https://www.activestate.com/)）获得</span><span class="sxs-lookup"><span data-stu-id="b007a-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="b007a-116">从源构建！</span><span class="sxs-lookup"><span data-stu-id="b007a-116">Build from source!</span></span>

<span data-ttu-id="b007a-117">除非有特定需求，否则建议使用前两个选项。</span><span class="sxs-lookup"><span data-stu-id="b007a-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="b007a-118">使用 pip 安装</span><span class="sxs-lookup"><span data-stu-id="b007a-118">Installation with pip</span></span>

<span data-ttu-id="b007a-119">可以单独安装每个 Azure 服务的库：</span><span class="sxs-lookup"><span data-stu-id="b007a-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="b007a-120">可以使用 `--pre` 标志安装预览包：</span><span class="sxs-lookup"><span data-stu-id="b007a-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="b007a-121">还可以使用 `azure` 元程序包在单个行中安装一组 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="b007a-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="b007a-122">我们发布了此包的预览版，可以使用 --pre 标志进行访问：</span><span class="sxs-lookup"><span data-stu-id="b007a-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="b007a-123">从 GitHub 安装</span><span class="sxs-lookup"><span data-stu-id="b007a-123">Install from GitHub</span></span>

<span data-ttu-id="b007a-124">如果想要从源安装 `azure`：</span><span class="sxs-lookup"><span data-stu-id="b007a-124">If you want to install `azure` from source:</span></span>

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a><span data-ttu-id="b007a-125">使用 pip 安装较旧版本</span><span class="sxs-lookup"><span data-stu-id="b007a-125">Install an older version with pip</span></span>
<span data-ttu-id="b007a-126">可以通过指定 'azure==3.0.0' 版本详细信息来安装 `azure` 的较旧版本。</span><span class="sxs-lookup"><span data-stu-id="b007a-126">You can install an older version of `azure` by specifying 'azure==3.0.0' version details.</span></span>
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a><span data-ttu-id="b007a-127">使用 pip 检查 SDK 安装详细信息</span><span class="sxs-lookup"><span data-stu-id="b007a-127">Check SDK installation details with pip</span></span>
<span data-ttu-id="b007a-128">可以检查 `azure` SDK 安装位置、版本详细信息等。</span><span class="sxs-lookup"><span data-stu-id="b007a-128">You can check `azure` SDK installation location, version details etc.</span></span>
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a><span data-ttu-id="b007a-129">使用 pip 进行卸载</span><span class="sxs-lookup"><span data-stu-id="b007a-129">To uninstall with pip</span></span>
<span data-ttu-id="b007a-130">可以使用 `azure` 元包在单个行中卸载所有 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="b007a-130">You can uninstall all Azure libraries in a single line using the `azure` meta-package.</span></span>
```bash
pip uninstall azure 
```
> [!NOTE]
> <span data-ttu-id="b007a-131">`pip uninstall azure` 删除 `azure` 元包但会留下个别 `azure-*` 包（以及其他包，如 `adal` 和 `msrest`）。</span><span class="sxs-lookup"><span data-stu-id="b007a-131">`pip uninstall azure`removes the `azure` meta-package but leaves the individual `azure-*` packages behind (and others, like `adal` and `msrest` ).</span></span> <span data-ttu-id="b007a-132">Python 和 pip 的一个方面是，对于具有依赖项的所有包，卸载初始包不会卸载依赖项。</span><span class="sxs-lookup"><span data-stu-id="b007a-132">An aspect of Python and pip is that for all packages that have dependencies, uninstalling the initial package does not uninstall the dependencies.</span></span> <span data-ttu-id="b007a-133">若要删除 `azure-` 及其支持包，请运行命令 `pip freeze | grep 'azure-' | xargs pip uninstall -y`（然后分别为 adal、msrest 和 msrestazureand 执行卸载。）</span><span class="sxs-lookup"><span data-stu-id="b007a-133">To remove `azure-` and its supporting packages, run the command `pip freeze | grep 'azure-' | xargs pip uninstall -y` (and then perform individual uninstalls for adal, msrest, and msrestazure).</span></span>

