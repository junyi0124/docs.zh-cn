---
ms.openlocfilehash: aba7b473af7685aa45f7e093a2f75311687593df
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506948"
---

如果收到类似于“找不到包 {dotnet-package}”或“无法安装某些包”的错误消息，请运行以下命令 。

以下命令组中有两个占位符。

- `{dotnet-package}`\
此项表示要安装的 .NET 包，如 `aspnetcore-runtime-3.1`。 它在以下 `sudo apt-get install` 命令中使用。

- `{os-version}`\
此项表示你所使用的 Linux 版本。 此项在以下 `wget` 命令中使用。

首先，尝试清除包列表：

```bash
sudo dpkg --purge packages-microsoft-prod && sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

然后，再次尝试安装 .NET。 如果这不起作用，可使用以下命令运行手动安装：
