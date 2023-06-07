---
title: NETSDK1071：“{0}”的 PackageReference 指定了版本 `{1}`。
description: 如何解决对某版本的框架包含的元包的 PackageReference 问题。
author: Forgind
ms.topic: error-reference
ms.date: 10/09/2020
f1_keywords:
- NETSDK1071
ms.openlocfilehash: 1381fc63941ec04efb31035d13913620a195c236
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713075"
---
# <a name="netsdk1071-explicitly-versioned-packagereference-to-a-metapackage-that-would-be-included-with-the-framework"></a>NETSDK1071：对框架中将包含的元包的显式版本化 PackageReference。

**本文适用于：** ✔️ .NET 5.0.100 SDK 及更高版本

当 .NET SDK 发出警告 NETSDK1071 时，它表明将来在 PackageReference 中指定的元包版本与通过 TargetFramework 属性隐式引用的元包版本之间可能会存在版本冲突：

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.1</TargetFramework>
</PropertyGroup>
```

由于 TargetFramework 会自动引入元包的版本，因此如果版本不同，它们将发生冲突。

若要解决此问题：

1. 在面向 .NET Core 或 .NET Standard 时，请考虑避免显式引用项目文件中的 `Microsoft.NETCore.App` 或 `NETStandard.Library`。
2. 面向 .NET Core 时，如果需要特定版本的运行时，请使用 `<RuntimeFrameworkVersion>` 属性，而不是直接引用元包。 例如，如果使用的是 [self-contained deployments](../../deploying/index.md#publish-self-contained) 并需要 1.0.0 LTS 运行时的特定修补程序，则可能会发生这种情况。
3. 面向 .NET Standard 时，如果需要特定版本的 `NetStandard.Library`，可以使用 `<NetStandardImplicitPackageVersion>` 属性并将其设置为所需版本。
4. 请勿在 .NET Framework 项目中显式添加或更新对 `Microsoft.NETCore.App` 或 `NETSTandard.Library` 的引用。 使用基于 .NET Standard 的 NuGet 包时，NuGet 会自动安装所需的任何版本的 `NETStandard.Library`。
5. 使用 .NET Core 2.1+ 时，请勿指定 `Microsoft.AspNetCore.App` 或 `Microsoft.AspNetCore.All` 版本，因为 .NET Core SDK 会自动选择相应的版本。 （注意：如果该项目还使用 `Microsoft.NET.Sdk.Web`，则此操作仅适用于面向 .NET Core 2.1 的情况。 .NET Core 2.2 SDK 中解决了此问题。）
6. 如果希望警告消失，还可以禁用它：

   ```xml
   <PackageReference Include="Microsoft.NetCore.App" Version="2.2.8" >
     <AllowExplicitVersion>true</AllowExplicitVersion>
   </PackageReference>
   ```
