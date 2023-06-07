---
ms.openlocfilehash: 9544b65f31772d0f4cee918528a73171fec4de99
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031968"
---
### <a name="change-in-default-value-of-useshellexecute"></a>UseShellExecute 默认值更改

<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType> 在 .NET Core 上的默认值为 `false`。 在 .NET Framework 上，其默认值为 `true`。

#### <a name="change-description"></a>更改描述

可以通过 <xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType> 直接启动应用程序，例如，使用 `Process.Start("mspaint.exe")` 代码启动画图。 如果 <xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType> 设置为 `true`，它还允许间接启动关联的应用程序。 在 .NET Framework 上，<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType> 的默认值 `true`，这意味着，如果已将 .txt  文件与该编辑器相关联，则 `Process.Start("mytextfile.txt")` 会启动记事本。 若要防止在 .NET Framework 上间接启动应用，必须将 <xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType> 显式设置为 `false`。 在 .NET Core 中，<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType> 的默认值为 `false`。 这意味着，默认情况下，在调用 `Process.Start` 时不会启动关联的应用程序。

出于性能方面的考虑，.NET Core 中引入了此更改。 通常情况下，<xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType> 用于直接启动应用程序。 直接启动应用并不需要使用 Windows shell，也不会产生关联的性能成本。 为了更快地使此情况默认化，.NET Core 将 <xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType> 的默认值更改为 `false`。 如果需要，可以选择慢速路径。

#### <a name="version-introduced"></a>引入的版本

2.1

> [!NOTE]
> 在早期版本的 .NET Core 中，没有为 Windows 实现 `UseShellExecute`。

#### <a name="recommended-action"></a>建议操作

如果应用依赖于旧行为，请调用 <xref:System.Diagnostics.Process.Start(System.Diagnostics.ProcessStartInfo)?displayProperty=nameWithType>，并将 <xref:System.Diagnostics.ProcessStartInfo.UseShellExecute> 设置为 <xref:System.Diagnostics.ProcessStartInfo> 对象上的 `true`。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.ProcessStartInfo?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Diagnostics.Process.Start`
- `M:System.Diagnostics.ProcessStartInfo`

-->
