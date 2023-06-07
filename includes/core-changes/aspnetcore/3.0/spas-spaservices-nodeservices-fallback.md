---
ms.openlocfilehash: e5355387d5cb6d9e6de89f5b85e64bc100b32ae1
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032434"
---
### <a name="spas-spaservices-and-nodeservices-no-longer-fall-back-to-console-logger"></a>SPA：SpaServices 和 NodeServices 不再回退到控制台记录器

除非配置了日志记录，否则 <xref:Microsoft.AspNetCore.SpaServices?displayProperty=nameWithType> 和 <xref:Microsoft.AspNetCore.NodeServices?displayProperty=nameWithType> 不会显示控制台日志。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`Microsoft.AspNetCore.SpaServices` 和 `Microsoft.AspNetCore.NodeServices` 用于在未配置日志记录时自动创建控制台记录器。

#### <a name="new-behavior"></a>新行为

除非配置了日志记录，否则 `Microsoft.AspNetCore.SpaServices` 和 `Microsoft.AspNetCore.NodeServices` 不会显示控制台日志。

#### <a name="reason-for-change"></a>更改原因

需要与其他 ASP.NET Core 包实现日志记录的方式保持一致。

#### <a name="recommended-action"></a>建议操作

如果需要旧行为，若要配置控制台日志记录，请将 `services.AddLogging(builder => builder.AddConsole())` 添加到 `Setup.ConfigureServices` 方法。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!--

#### Affected APIs

Not detectable via API analysis

-->
