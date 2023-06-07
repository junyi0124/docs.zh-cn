---
title: .NET 库的跨平台定位
description: 有关创建跨平台 .NET 库的最佳做法建议。
ms.date: 08/12/2019
ms.openlocfilehash: 038a03904c4cfe49758562b5748fef06ae1afa4b
ms.sourcegitcommit: 7588b1f16b7608bc6833c05f91ae670c22ef56f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2020
ms.locfileid: "93189246"
---
# <a name="cross-platform-targeting"></a>跨平台定位

新式 .NET 支持多个操作系统和设备。 .NETNET 开源库支持尽可能多的开发人员（无论是构建 Azure 中托管的 ASP.NET 网站的开发人员，还是在 Unity 中开发 .NET 游戏的开发人员），这一点非常重要。

## <a name="net-standard"></a>.NET Standard

.NET Standard 是为 .NET 库启用跨平台支持的最佳方式。 [.NET Standard](../net-standard.md) 是一套 .NET API 规范，在所有 .NET 实现中推出。 以 .NET Standard 为目标可以生成受限于使用给定版本的 .NET Standard 中的 API 的库，这意味着实现该版本的 .NET Standard 的所有平台都可以使用它。

![.NET Standard](./media/cross-platform-targeting/platforms-netstandard.png ".NET Standard")

面向 .NET Standard 并且成功编译项目后，并不能保证库可以在所有平台上成功运行：

1. 特定于平台的 API 将在其他平台上失败。 例如，<xref:Microsoft.Win32.Registry?displayProperty=nameWithType> 可以在 Windows 上成功运行，但在任何其他操作系统上使用时，会引发 <xref:System.PlatformNotSupportedException>。
2. API 的行为可能有所不同。 例如，当应用程序在 iOS 或 UWP 上使用提前编译时，反射 API 具有不同的性能特征。

> [!TIP]
> .NET 团队[提供了 Roslyn 分析器](../analyzers/api-analyzer.md)，可帮助你发现可能存在的问题。

✔️ 请务必首先包含一个 `netstandard2.0` 目标。

> 最常规用途的库应该不需要除 .NET Standard 2.0 之外的其他 API。 所有新式平台都支持 .NET Standard 2.0，并且它是支持具有一个目标的多个平台的推荐方法。

❌ 请避免包含 `netstandard1.x` 目标。

> .NET Standard 1.x 作为一组精细的 NuGet 包分发，它创建了一个大型的包依赖项关系图，并导致开发人员在构建时下载大量的包。 新式 .NET 实现支持 .NET Standard 2.0。 如果需要专门面向较旧的平台，则只能面向 .NET Standard。

✔️ 如果需要面向 `netstandard1.x` 目标，请务必包括 `netstandard2.0` 目标。

> 所有支持 .NET Standard 2.0 的平台都将使用 `netstandard2.0` 目标，并从较小的包关系图中受益，而较旧的平台仍然可以正常运行并回退到使用 `netstandard1.x` 目标。

❌ 如果库依赖于平台特定的应用模型，请避免包括 .NET Standard 目标。

> 例如，UWP 控件工具包库依赖的应用模型只能在 UWP 上使用。 特定于应用模型的 API 将无法在 .NET Standard 中使用。

## <a name="multi-targeting"></a>多目标

有时，需要从库中访问特定于框架的 API。 调用特定于框架的 API 的最佳方法是使用多目标，它为许多（而不是仅为一个）[.NET 目标框架](../frameworks.md)构建项目。

为了让使用者不必针对各个框架构建，应该尽量获取 .NET Standard 输出以及一个或多个特定于框架的输出。 通过多目标，所有程序集都打包在一个 NuGet 包中。 然后，使用者可以引用同一个包，NuGet 将选择适当的实现。 你的 .NET Standard 库充当在任何地方使用的回退库（NuGet 包提供特定于框架的实现的情况除外）。 通过多目标可以在代码中使用条件编译并调用特定于框架的 API。

![包含多个程序集的 NuGet 包](./media/cross-platform-targeting/nuget-package-multiple-assemblies.png "包含多个程序集的 NuGet 包")

✔️ 请在面向 .NET Standard 的基础上考虑面向 .NET 实现。

> 通过面向 .NET 实现，可以调用 .NET Standard 范围之外的特定于平台的 API。
>
> 执行此操作时，请不要放弃对 .NET Standard 的支持。 相反，从实现引发并提供功能 API。 这样一来，你的库可以在任何地方使用，并支持运行时启动功能。

```csharp
public static class GpsLocation
{
    // This project uses multi-targeting to expose device-specific APIs to .NET Standard.
    public static async Task<(double latitude, double longitude)> GetCoordinatesAsync()
    {
#if NET461
        return CallDotNetFramworkApi();
#elif WINDOWS_UWP
        return CallUwpApi();
#else
        throw new PlatformNotSupportedException();
#endif
    }

    // Allows callers to check without having to catch PlatformNotSupportedException
    // or replicating the OS check.
    public static bool IsSupported
    {
        get
        {
#if NET461 || WINDOWS_UWP
            return true;
#else
            return false;
#endif
        }
    }
}
```

❌ 如果你的源代码源代码对所有目标都相同，请避免多目标以及面向 .NET Standard。

> .NET Standard 程序集将自动供 NuGet 使用。 面向单个 .NET 实现会增加 `*.nupkg` 大小，不会提供任何好处。

✔️ 请考虑在提供 `netstandard2.0` 目标时为 `net461` 添加目标。

> 在 .NET Framework 中使用 .NET Standard 2.0 存在一些问题，这些在 .NET Framework 4.7.2 中已得到解决。 可以为仍在使用 .NET Framework 4.6.1 - 4.7.1 的开发人员提供为 .NET Framework 4.6.1 构建的二进制文件，从而改善其体验。

✔️ 请使用 NuGet 包分发库。

> NuGet 将为开发人员选择最佳目标，使他们无需选择适当实现。

✔️ 请务必在使用多目标时使用项目文件的 `TargetFrameworks` 属性。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <!-- This project will output netstandard2.0 and net461 assemblies -->
    <TargetFrameworks>netstandard2.0;net461</TargetFrameworks>
  </PropertyGroup>
</Project>
```

✔️ 请考虑在对 UWP 和 Xamarin 进行多目标定位时使用 [MSBuild.Sdk.Extras](https://github.com/onovotny/MSBuildSdkExtras)，它可以极大地简化项目文件。

## <a name="older-targets"></a>较旧的目标

.NET 支持长期不受支持的 .NET Framework 的目标版本以及不再经常使用的平台。 尽管使库在尽可能多的目标上工作是有价值的，但这种情况下必须解决缺少的 API 问题，这会增加很大的开销。 考虑到某些框架的使用范围和局限性，我们认为不再值得面向这些框架。

❌ 请避免包括可移植类库 (PCL) 目标。 例如，`portable-net45+win8+wpa81+wp8` 。

> .NET standard 是支持跨平台 .NET 库的新式方法，它可以替代 PCL。

❌ 请避免包括面向不再受支持的 .NET 平台的目标。 例如：`SL4`、`WP`。

>[!div class="step-by-step"]
>[上一页](get-started.md)
>[下一页](strong-naming.md)
