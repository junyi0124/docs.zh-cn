---
ms.openlocfilehash: 53cac9ca49502c88f5d3cf63bf113365e7b85d18
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94507038"
---

### <a name="install-the-sdk"></a>安装 SDK

.NET Core SDK 使你可以通过 .NET Core 开发应用。 如果安装 .NET Core SDK，则无需安装相应的运行时。 若要安装 .NET Core SDK，请运行以下命令：

```bash
sudo dnf install dotnet-sdk-3.1
```

### <a name="install-the-runtime"></a>安装运行时

.NET Core 运行时允许运行使用不随附运行时的 .NET Core 所开发的应用。 以下命令安装 ASP.NET Core 运行时，这是与 .NET Core 最兼容的运行时。 在终端中，运行以下命令。

```bash
sudo dnf install aspnetcore-runtime-3.1
```

作为 ASP.NET Core 运行时的一种替代方法，你可以安装不包含 ASP.NET Core 支持的 .NET Core 运行时：将上一命令中的 `aspnetcore-runtime-3.1` 替换为 `dotnet-runtime-3.1`。

```bash
sudo dnf install dotnet-runtime-3.1
```
