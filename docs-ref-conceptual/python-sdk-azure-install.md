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
ms.openlocfilehash: 9fd11cbc7b987b970ceee85c7b11b22e3d6299ea
ms.sourcegitcommit: 31d7df367b15ec09a5a610eb333295bba0f6b351
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2019
ms.locfileid: "67395451"
---
# <a name="installation"></a>安装

## <a name="which-python-and-which-version-to-use"></a>要使用哪种 Python 以及哪个版本

有多个 Python 解释程序可用 - 示例包括：

* CPython - 最常用的标准 Python 解释程序
* PyPy - 快速、CPython 的合规替代实现
* IronPython - 在 .Net/CLR 上运行的 Python 解释程序
* Jython - 在 Java 虚拟机上运行的 Python 解释程序

**CPython** v2.7 或 v3.4+ 和 PyPy 5.4.0 已经过测试，并且受 Python Azure SDK 支持。

## <a name="where-to-get-python"></a>从哪里获得 Python？

有多种方法可获得 CPython：

* 直接从 [Python](https://www.python.org/) 获得
* 从可信发行版（如[Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/) 或 [ActiveState](https://www.activestate.com/)）获得
* 从源构建！

除非有特定需求，否则建议使用前两个选项。

## <a name="installation-with-pip"></a>使用 pip 安装

可以单独安装每个 Azure 服务的库：

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

可以使用 `--pre` 标志安装预览包：

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

还可以使用 `azure` 元程序包在单个行中安装一组 Azure 库。

```bash
pip install azure
```

我们发布了此包的预览版，可以使用 --pre 标志进行访问：

```bash
pip install --pre azure
```

## <a name="install-from-github"></a>从 GitHub 安装

如果想要从源安装 `azure`：

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a>使用 pip 安装较旧版本
可以通过指定 'azure==3.0.0' 版本详细信息来安装 `azure` 的较旧版本。
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a>使用 pip 检查 SDK 安装详细信息
可以检查 `azure` SDK 安装位置、版本详细信息等。
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a>使用 pip 进行卸载
可以使用 `azure` 元包在单个行中卸载所有 Azure 库。
```bash
pip uninstall azure 
```
> [!NOTE]
> `pip uninstall azure` 删除 `azure` 元包但会留下个别 `azure-*` 包（以及其他包，如 `adal` 和 `msrest`）。 Python 和 pip 的一个方面是，对于具有依赖项的所有包，卸载初始包不会卸载依赖项。 若要删除 `azure-` 及其支持包，请运行命令 `pip freeze | grep 'azure-' | xargs pip uninstall -y`（然后分别为 adal、msrest 和 msrestazureand 执行卸载。）

