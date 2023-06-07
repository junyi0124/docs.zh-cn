---
title: 依赖项和.NET 库
description: 管理 .NET 库中 NuGet 依赖项的最佳做法建议。
ms.date: 10/02/2018
ms.openlocfilehash: d189a3364b501272e29de72b6018844877bf2128
ms.sourcegitcommit: 7588b1f16b7608bc6833c05f91ae670c22ef56f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2020
ms.locfileid: "93189298"
---
# <a name="dependencies"></a>依赖项

向 .NET 库添加依赖项的主要方法是引用 NuGet 包。 通过 NuGet 包引用，可以快速重用和利用已编写的功能，但它们是 .NET 开发人员产生冲突的一个常见原因。 请务必正确管理依赖项，这可以防止其他 .NET 库中的更改破坏你的 .NET 库，反之亦然！

## <a name="diamond-dependencies"></a>菱形依赖关系

.NET 项目在其依赖关系树中具有多个版本的包，这种情况非常常见。 例如，应用程序依赖于两个 NuGet 包，每个包依赖于同一包的不同版本。 应用的依赖项关系图中现在存在菱形依赖关系。

![菱形依赖关系](./media/dependencies/diamond-dependency.png "菱形依赖关系")

在构建时，NuGet 会分析项目依赖的所有包，包括依赖项的依赖项。 检测到包的多个版本时，将评估规则以便选择一个版本。 统一包是必要的，因为如果在同一个应用程序中运行某个程序集的并行版本，在 .NET 中会出现问题。

大多数菱形依赖关系很容易解决；但是，在某些情况下，它们可能会引发问题：

- 存在冲突的 NuGet 包引用会导致在包还原期间无法解析版本。
- 各版本之间的重大更改会在运行时导致 bug 和异常。
- 包程序集具有强名称，程序集版本会发生更改，并且在 .NET Framework 上运行应用。 程序集绑定重定向是必需的。

无法确定哪些包将与你自己的包一起使用。 降低菱形依赖关系破坏库的可能性的一个好方法是将依赖的包的数量降至最低。

✔️ 请务必查看 .NET 库中是否存在不必要的依赖项。

## <a name="nuget-dependency-version-ranges"></a>NuGet 依赖项版本范围

包引用指定允许的有效包的范围。 通常情况下，项目文件中的包引用版本是最小版本，并且不存在最大版本。

```xml
<!-- Accepts any version 1.0 and above. -->
<PackageReference Include="ExamplePackage" Version="1.0" />
```

NuGet 在解析依赖关系时使用的规则是 [complex](/nuget/consume-packages/dependency-resolution)，但[默认情况下](/nuget/consume-packages/install-use-packages-visual-studio#install-and-update-options)，NuGet 始终查找适用的最低版本。 NuGet 倾向于使用最低适用版本，而不使用最高版本，因为最低版本的兼容性问题最少。

鉴于 NuGet 的最低适用版本规则，没有必要在包引用时放置较高版本或确切范围，以避免获取最新版本。 NuGet 已经尝试为你查找兼容性最高的最低版本。

```xml
<!-- Accepts 1.0 up to 1.x, but not 2.0 and higher. -->
<PackageReference Include="ExamplePackage" Version="[1.0,2.0)" />

<!-- Accepts exactly 1.0. -->
<PackageReference Include="ExamplePackage" Version="[1.0]" />
```

如果存在冲突，较高版本限制将导致 NuGet 失败。 例如，一个库接受 1.0 版本，而另一个库需要 2.0 或更高版本。 虽然版本 2.0 中可能引入了重大更改，但严格的或上限版本依赖项绝对会出现错误。

![菱形依赖关系冲突](./media/dependencies/diamond-dependency-conflict.png "菱形依赖关系冲突")

❌ 请避免执行无最低版本的 NuGet 包引用。

❌ 请避免使用需要确切版本的 NuGet 包引用。

❌ 请避免执行存在版本上限的 NuGet 包引用。

## <a name="nuget-shared-source-packages"></a>NuGet 共享源包

减少外部 NuGet 包依赖项的一种方法是引用共享源包。 共享源包包含引用时项目中所含的[源代码文件](/nuget/reference/nuspec#including-content-files)。 由于只包括使用项目的其余部分编译的源代码文件，因此，没有外部依赖项，也没有存在冲突的可能性。

共享源包非常适合包含一些小功能。 例如，用于进行 HTTP 调用的帮助程序方法的共享源包。

![共享源包](./media/dependencies/shared-source-package.png "共享源包")

```xml
<PackageReference Include="Microsoft.Extensions.Buffers.Testing.Sources" PrivateAssets="All" Version="1.0" />
```

![共享源项目](./media/dependencies/shared-source-project.png "共享源项目")

共享源包存在一些限制。 它们只能由 `PackageReference` 引用，因此排除了较旧的 `packages.config` 项目。 另外，共享源包只能由具有相同语言类型的项目使用。 由于这些限制，共享源包最好用于在开源项目中共享功能。

✔️ 请考虑引用共享源包并用于一些小型的内部功能。

✔️ 请考虑在包提供一些小型内部功能时将其用作共享源包。

✔️ 请务必使用 `PrivateAssets="All"` 引用共享源包。

> 此项设置将告知 NuGet：包只在开发时使用，不应作为公共依赖项公开。

❌ 请避免在公共 API 中包含共享源包类型。

> 共享源类型将编译到引用程序集中，并且无法跨程序集边界进行交换。 例如，一个项目中的共享源 `IRepository` 类型与另一个项目中的同一共享源 `IRepository` 是彼此独立的类型。 共享源包中的类型应具有 `internal` 可见性。

❌ 请避免将共享源包发布到 NuGet.org。

> 共享源包包含源代码，只能由具有相同语言类型的项目使用。 例如，F# 应用程序无法使用 C# 共享源包。
>
> 发布共享源包到[本地数据源或 MyGet](./publish-nuget-package.md)，以在项目内部使用它们。

>[!div class="step-by-step"]
>[上一页](nuget.md)
>[下一页](sourcelink.md)
