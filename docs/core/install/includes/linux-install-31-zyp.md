---
ms.openlocfilehash: 36f1ee26def82d426b6637ae96f382fc89791a2f
ms.sourcegitcommit: b1442669f1982d3a1cb18ea35b5acfb0fc7d93e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93135757"
---

### <a name="install-the-sdk"></a>安装 SDK

.NET Core SDK 使你可以通过 .NET Core 开发应用。 若要安装 .NET Core SDK，请运行以下命令。

```bash
sudo zypper install dotnet-sdk-3.1
```

### <a name="install-the-runtime"></a>安装运行时

.NET Core 运行时允许运行使用不随附运行时的 .NET Core 所开发的应用。 以下命令安装 ASP.NET Core 运行时，这是与 .NET Core 最兼容的运行时。 在终端中，运行以下命令。

```bash
sudo zypper install aspnetcore-runtime-3.1
```

作为 ASP.NET Core 运行时的一种替代方法，你可以安装不包含 ASP.NET Core 支持的 .NET Core 运行时：将上一命令中的 `aspnetcore-runtime-2.1` 替换为 `dotnet-runtime-3.1`。

```bash
sudo zypper install dotnet-runtime-3.1
```
