---
title: 缓解：产品版本控制
description: 在本文中，了解 .NET Framework 4.6 和更高版本产品版本控制与早期版本相比有何变化。
ms.date: 03/30/2017
ms.assetid: 1c4de9d7-9aba-427a-8f38-0ab9bfb8f85e
ms.openlocfilehash: 442c06446e763758d3a150ee9ff884a616541c07
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475393"
---
# <a name="mitigation-product-versioning"></a>缓解：产品版本控制

在 .NET Framework 4.6 和更高版本中，产品版本控制已更改，不再是早期版本的 .NET Framework（.NET Framework 4、4.5、4.5.1 和 4.5.2）。

## <a name="product-versioning-changes"></a>产品版本控制更改

以下是详细更改：

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` 键的 `Version` 项的值在 .NET Framework 4.6 及其点版本中已更改为 `4.6.`*xxxxx*，在 .NET Framework 4.7 中已更改为 `4.7.`*xxxxx*。 在 .NET Framework 4.5、4.5.1 和 4.5.2 中，它的格式为 `4.5.`*xxxxx*。

- .NET Framework 文件的文件和产品版本控制在 .NET Framework 4.6 及其点版本中已从 `4.0.30319.x` 的早期版本控制方案更改为 `4.6.X.0`，在 .NET Framework 4.7 及其点版本中已更改为 `4.7.X.0`。 右键单击某个文件后，查看文件的“属性”时可以看到这些新值。

- 对于 .NET Framework 4.6 及其点版本，托管程序集的 <xref:System.Reflection.AssemblyFileVersionAttribute> 和 <xref:System.Reflection.AssemblyInformationalVersionAttribute> 特性具有格式为 `4.6.X.0` 的 <xref:System.Version> 值，对于 .NET Framework 4.7，则为格式 `4.7.X.0`。

- 从 .NET Framework 4.6 开始，<xref:System.Environment.Version%2A?displayProperty=nameWithType> 属性将返回修正后的版本字符串 `4.0.30319.42000`。 在 .NET Framework 4、4.5、4.5.1 和 4.5.2 中，它返回格式为 `4.0.30319.xxxxx`（其中 `xxxxx` 小于 42000）的版本字符串，例如“4.0.30319.18010”。 请注意，我们不建议应用程序代码对 <xref:System.Environment.Version%2A?displayProperty=nameWithType> 属性产生任何新的依赖关系。

### <a name="handling-the-product-versioning-changes"></a>处理产品版本控制更改

一般情况下，应用程序应依赖于用于检测诸如 .NET Framework 的运行时版本和安装目录等内容的推荐技术：

- 要检测 .NET Framework 的运行时版本，请参阅[如何：确定已安装的 .NET Framework 版本](how-to-determine-which-versions-are-installed.md)。

- 若要确定 .NET Framework 的安装路径，请使用 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` 密钥中的 `InstallPath` 条目的值。

  > [!IMPORTANT]
  > 子项名称是 `NET Framework Setup`，而不是 `.NET Framework Setup`。

- 若要确定.NET Framework 公共语言运行时的目录路径，请调用 <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory%2A?displayProperty=nameWithType> 方法。

- 若要获取 CLR 版本，请调用 <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion%2A?displayProperty=nameWithType> 方法。   对于 .NET Framework 4 及其子版本（.NET Framework 4.5、4.5.1、4.5.2 以及 .NET Framework 4.6、4.6.1、4.6.2 和 4.7），它将返回字符串 `v4.0.30319`。

## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
