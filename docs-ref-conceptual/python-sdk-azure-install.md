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
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a>安装

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

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
