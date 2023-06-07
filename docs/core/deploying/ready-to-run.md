---
title: ReadyToRun 部署概述
description: 了解什么是 ReadyToRun 部署，以及通过 .NET 5 和 .NET Core 3.0 及更高版本发布应用时为什么应考虑使用它。
author: davidwr
ms.author: davidwr
ms.date: 09/21/2020
ms.openlocfilehash: cd8eaebd05d79b11e90e255e702a52220fffda76
ms.sourcegitcommit: ffd4d5e824db6c5f0c3521c0e802fd9e8f0edcbe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93342626"
---
# <a name="readytorun-compilation"></a>ReadyToRun 编译

可以通过将应用程序集编译为 ReadyToRun (R2R) 格式来改进 .NET Core 应用程序的启动时间和延迟。 R2R 是一种预先 (AOT) 编译形式。

R2R 二进制文件通过减少应用程序加载时实时 (JIT) 编译器需要执行的工作量来改进启动性能。 二进制文件包含与 JIT 将生成的内容类似的本机代码。 但是，R2R 二进制文件更大，因为它们包含中间语言 (IL) 代码（某些情况下仍需要此代码）和相同代码的本机版本。 仅当发布面向特定运行时环境 (RID)（如 Linux x64 或 Windows x64）的应用时 R2R 才可用。

若要将项目编译为 ReadyToRun，必须在将 PublishReadyToRun 属性设置为 true 时发布应用程序。

可以通过两种方式将应用发布为 ReadyToRun：

01. 向 dotnet publish 命令直接指定 PublishReadyToRun 标志。 有关详细信息，请参阅 [dotnet publish](../tools/dotnet-publish.md)。

    ```dotnetcli
    dotnet publish -c Release -r win-x64 -p:PublishReadyToRun=true
    ```

02. 在项目中指定属性

    - 向项目中添加 `<PublishReadyToRun>` 设置。

    ```xml
    <PropertyGroup>
      <PublishReadyToRun>true</PublishReadyToRun>
    </PropertyGroup>
    ```

    - 在不使用任何特殊参数的情况下发布应用程序。

    ```dotnetcli
    dotnet publish -c Release -r win-x64
    ```

## <a name="impact-of-using-the-readytorun-feature"></a>使用 ReadyToRun 功能的影响

预先编译会对应用程序性能产生复杂的性能影响，这种影响可能很难预测。 通常情况下，程序集的大小将增长到两到三倍。 这种文件物理大小的增加可能会降低从磁盘加载程序集的性能，并增加进程的工作集。 但相对地，在运行时编译的方法数通常会大幅减少。 因此，启用 ReadyToRun，包含大量代码的大多数应用程序都会获得很大的性能增益。 由于 .NET 运行时库已使用 ReadyToRun 进行预编译，因此启用 ReadyToRun 时，具有少量代码的应用程序很可能不会获得显著改进。

这里讨论的启动改进不仅适用于应用程序启动，还适用于在应用程序中首次使用任何代码。 例如，ReadyToRun 可用于降低在 ASP.NET 应用程序中首次使用 Web API 时的响应延迟。

### <a name="interaction-with-tiered-compilation"></a>通过分层编译进行交互

预先生成代码优化程度不如 JIT 生成的代码。 为了解决此问题，分层编译将使用 JIT 生成的方法替换常用的 ReadyToRun 方法。

## <a name="how-is-the-set-of-precompiled-assemblies-chosen"></a>如何选择预编译的一组程序集？

SDK 将预编译随应用程序一起分发的程序集。 对于独立应用程序，这组程序集将包括框架。 C++/CLI 二进制文件不适合 ReadyToRun 编译。

## <a name="how-is-the-set-of-methods-to-precompile-chosen"></a>如何选择要预编译的一组方法？

编译器会尝试预编译尽可能多的方法。 但由于各种原因，不应预期使用 ReadyToRun 功能会导致无法执行 JIT。

这些原因包括（但不限于）：

- 使用在单独的程序集中定义的泛型类型
- 与本机代码互操作
- 使用编译器无法证明可在目标计算机上安全使用的硬件内部函数
- 某些异常 IL 模式
- 通过反射或 LINQ 创建动态方法

## <a name="cross-platformarchitecture-restrictions"></a>跨平台/体系结构限制

对于某些 SDK 平台，ReadyToRun 编译器可以进行针对其他目标平台的交叉编译。 下表介绍了支持的编译目标。

| SDK 平台 | 支持的目标平台 |
| ------------ | --------------------------- |
| Windows X64  | Windows X86、Windows X64、Windows ARM32、Windows ARM64 |
| Windows X86  | Windows X86、Windows ARM32 |
| Linux X64    | Linux X86、Linux X64、Linux ARM32、Linux ARM64 |
| Linux ARM32  | Linux ARM32 |
| Linux ARM64  | Linux ARM64 |
| macOS X64    | macOS X64 |
