---
title: 如何对 .NET 运行时和 SDK 进行版本控制
description: 本文介绍如何对 .NET SDK 和运行时进行版本控制（与语义版本控制类似）。
ms.date: 12/07/2020
ms.openlocfilehash: 2fbc2775f31b4eab1c9883282c58accd9bb2b9f5
ms.sourcegitcommit: 635a0ff775d2447a81ef7233a599b8f88b162e5d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97633619"
---
# <a name="overview-of-how-net-is-versioned"></a>.NET 的版本控制方式概述

[.NET 运行时和 .NET SDK](../introduction.md#sdk-and-runtimes) 添加新功能的频率不同。 通常来说，SDK 的更新频率比运行时高。 本文介绍运行时和 SDK 版本号。

## <a name="versioning-details"></a>版本控制详细信息

.NET 运行时具有用于版本控制的主要/次要/补丁方法，且该方法遵循[语义版本控制](#semantic-versioning)。

.NET SDK 不遵循语义版本控制。 .NET SDK 发布速度更快，其版本必须显示相应的运行时和 SDK 自己的次要版本及补丁版本。

.NET SDK 版本号的前两个位置被锁定，其中显示与之一起发布的 .NET 运行时版本。 每个版本的 SDK 都可以为此版本或任何更低版本的运行时创建应用程序。

SDK 版本号的第三个位置同时传达次要编号和修补程序编号。 次要版本乘以 100。 最后两位数代表修补程序号。 次要版本 1，修补程序版本 2 将表示为 102。 例如，下面是运行时和 SDK 版本号可能出现的序列：

| 更改                | .NET 运行时      | .NET SDK (\*)     |
|-----------------------|-------------------|-------------------|
| 初始版本       | 2.2.0             | 2.2.100           |
| SDK 修补程序             | 2.2.0             | 2.2.101           |
| 运行时和 SDK 修补程序 | 2.2.1             | 2.2.102           |
| SDK 功能更改    | 2.2.1             | 2.2.200           |

注意：

- 如果在运行时功能更新之前，SDK 有 10 个功能更新，则版本号将滚动到 1000 系列，2.2.1000 等编号为 2.2.900 之后的功能版本。 应该不会出现这种情况。
- 不会出现为发布功能的 99 修补程序版本。 如果某版本接近此数字，则会强制发布功能。

可在 [dotnet/设计](https://github.com/dotnet/designs/pull/29) 存储库中查看初始建议的更多详细信息。

## <a name="semantic-versioning"></a>语义化版本控制

.NET 运行时大致遵循[语义版本控制 (SemVer)](https://semver.org/)，采用 `MAJOR.MINOR.PATCH` 版本控制，通过版本号的各部分来描述更改程度和类型。

```
MAJOR.MINOR.PATCH[-PRERELEASE-BUILDNUMBER]
```

可选的 `PRERELEASE` 和 `BUILDNUMBER` 部分永远不会成为受支持版本的一部分，并且将仅存在于夜间版本、来自源目标的本地版本，以及不受支持的预览版本中。

### <a name="understand-runtime-version-number-changes"></a>了解运行时版本号更改

`MAJOR` 在下列情况时递增：

- 产品或新产品方向发生重大更改。
- 发生了中断性变更。 接受中断性变更存在较大障碍。
- 旧版本不再受支持。
- 采用了现有依赖项的较新 `MAJOR` 版本。

`MINOR` 在下列情况时递增：

- 添加了公共 API 外围应用。
- 添加了新行为。
- 采用了现有依赖项的较新 `MINOR` 版本。
- 引入了新依赖项。

`PATCH` 在下列情况时递增：

- 进行了 Bug 修复。
- 添加了对较新平台的支持。
- 采用了现有依赖项的较新 `PATCH` 版本。
- 任何其他不符合上述情况的更改。

存在多处更改时，单个更改影响的最高级别元素会递增，并将随后的元素重置为零。 例如，当 `MAJOR` 递增时，`MINOR` 和 `PATCH` 将重置为零。 当 `MINOR` 递增时，`PATCH` 将重置为零，而 `MAJOR` 保持不变。

## <a name="version-numbers-in-file-names"></a>文件名中的版本号

为 .NET 下载的文件带有版本，例如 `dotnet-sdk-2.1.300-win10-x64.exe`。

### <a name="preview-versions"></a>预览版

预览版向版本号追加了 `-preview[number]-([build]|"final")`。 例如 `2.0.0-preview1-final`。

### <a name="servicing-versions"></a>服务版本

在版本发布后，版本分支通常停止生成日常版本，而开始生成服务版本。 服务版本向版本追加了 `-servicing-[number]`。 例如 `2.0.1-servicing-006924`。

## <a name="relationship-to-net-standard-versions"></a>与 .NET Standard 版本的关系

.NET Standard 由 .NET 引用程序集组成。 每个平台都有多个特定的实现。 引用程序集包含 .NET API 的定义，后者是给定 .NET Standard 版本的一部分。 每个实现均满足特定平台上的 .NET Standard 协定。

.NET Standard 引用程序集使用 `MAJOR.MINOR` 版本控制方案。 `PATCH` 级别对 .NET Standard 并无用处，因为它只公开 API 规范（没有实现），并且根据定义，对 API 的任何更改都将作为功能集的更改，从进而产生新的 `MINOR` 版本。

每个平台上的实现通常可作为平台版本的一部分更新，因此对于在该平台上使用 .NET Standard 的程序员来说并不明显。

有关详细信息，请参阅 [.NET Standard](../../standard/net-standard.md)。

## <a name="see-also"></a>请参阅

- [目标框架](../../standard/frameworks.md)
- [.NET 分发打包](../distribution-packaging.md)
- [.NET 支持生命周期简报](https://dotnet.microsoft.com/platform/support/policy)
- [.NET 的 Docker 映像](https://hub.docker.com/_/microsoft-dotnet/)
