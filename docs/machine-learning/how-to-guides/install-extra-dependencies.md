---
title: 安装额外的 ML.NET 依赖项
description: 了解如何安装 ML.NET 包所依赖的但又不与 NuGet 包一起安装的任何本机库
ms.date: 04/02/2020
author: natke
ms.author: nakersha
ms.custom: how-to
ms.openlocfilehash: 75d29c6bafdce5c9bb104229ddc8d7b847f57e29
ms.sourcegitcommit: fcbe432482464b1639decad78cc4dc8387c6269e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2020
ms.locfileid: "97366796"
---
# <a name="install-extra-mlnet-dependencies"></a>安装额外的 ML.NET 依赖项

在大多数情况下，在所有操作系统上，安装 ML.NET 就像引用适当的 NuGet 包一样简单。

```dotnetcli
dotnet add package Microsoft.ML
```

但在某些情况下，还存在其他安装要求，尤其是在需要本机组件时。 本文档介绍了这些情况的安装要求。 这些部分根据特定的 `Microsoft.ML.*` NuGet 包进行了细分，此包具有其他依赖项。

## <a name="microsoftmltimeseries-microsoftmlautoml"></a>Microsoft.ML.TimeSeries, Microsoft.ML.AutoML

这两个包都依赖于 `Microsoft.ML.MKL.Redist`，而后者又依赖于 `libomp`。

### <a name="windows"></a>Windows

无需执行额外的安装步骤。 将 NuGet 包添加到项目时，会安装库。

### <a name="linux"></a>Linux

1. 安装存储库的 GPG 键

    ```bash
    sudo bash
    # <type your user password when prompted.  this will put you in a root shell>
    # cd to /tmp where this shell has write permission
    cd /tmp
    # now get the key:
    wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    # now install that key
    apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    # now remove the public key file exit the root shell
    rm GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    exit
    ```

2. 为 MKL 添加 APT 存储库

    ```bash
    sudo sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list'
    ```

3. 更新包

    ```bash
    sudo apt-get update
    ```

4. 安装 MKL

    ```bash
    sudo apt-get install <COMPONENT>-<VERSION>.<UPDATE>-<BUILD_NUMBER>
    ```

    例如：

    ```bash
    sudo apt-get install intel-mkl-64bit-2020.0-088
    ```

    确定 `libiomp.so` 的位置

    ```bash
    find /opt -name "libiomp5.so"
    ```

    例如：

    ```output
    /opt/intel/compilers_and_libraries_2020.0.166/linux/compiler/lib/intel64_lin/libiomp5.so
    ```

5. 将此位置添加到加载库路径：

    ```bash
    sudo ldconfig /opt/intel/compilers_and_libraries_2020.0.166/linux/compiler/lib/intel64_lin
    ```

### <a name="mac"></a>Mac

1. 使用 `Homebrew` 安装库

    ```bash
    brew update && brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/f5b1ac99a7fba27c19cee0bc4f036775c889b359/Formula/libomp.rb && brew link libomp --force
    ```
