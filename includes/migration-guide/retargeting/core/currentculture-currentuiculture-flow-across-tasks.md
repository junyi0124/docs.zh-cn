---
ms.openlocfilehash: 78faa5f4008b41bac75c94ce09a58c8227e5b485
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614334"
---
### <a name="currentculture-and-currentuiculture-flow-across-tasks"></a>CurrentCulture 和 CurrentUICulture 在任务之间流动

#### <a name="details"></a>详细信息

从 .NET Framework 4.6 开始，<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 和 <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> 存储在线程的 <xref:System.Threading.ExecutionContext?displayProperty=fullName> 中，可跨异步操作流动。这意味着对 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 或 <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> 的更改将反映在稍后以异步方式运行的任务中。 这与旧版 .NET Framework 的行为不同（旧版会在所有异步任务中重置 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 和 <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName>）。

#### <a name="suggestion"></a>建议

受此更改影响的应用可通过将所需的 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 或 <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> 显式设置为异步任务中的首个操作来暂时解决此问题。 或者，可通过设置以下兼容性开关选择使用旧行为（不流动 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName>）：

```csharp
AppContext.SetSwitch("Switch.System.Globalization.NoAsyncCurrentCulture", true);
```

.NET Framework 4.6.2 中的 WPF 已修复此问题。 .NET Frameworks 4.6、4.6.1 中也通过 [KB 3139549](https://support.microsoft.com/kb/3139549) 修复了此问题。 面向 .NET Framework 4.6 或更高版本的应用程序将自动在 WPF 应用程序中获取正确行为 - <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> 将在调度程序操作中保留。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType>
- <xref:System.Threading.Thread.CurrentCulture?displayProperty=nameWithType>
- <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=nameWithType>
- <xref:System.Threading.Thread.CurrentUICulture?displayProperty=nameWithType>
