---
ms.openlocfilehash: 1c9c899d77dd69e185281d98bfec18ce73d80815
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032284"
---
### <a name="kestrel-empty-https-assembly-removed"></a>Kestrel：删除了空 HTTPS 程序集

已删除程序集 <xref:Microsoft.AspNetCore.Server.Kestrel.Https?displayProperty=fullName>。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="reason-for-change"></a>更改原因

在 ASP.NET Core 2.1 中，`Microsoft.AspNetCore.Server.Kestrel.Https` 的内容已移动到 <xref:Microsoft.AspNetCore.Server.Kestrel.Core?displayProperty=fullName> 中。 此更改是使用 `[TypeForwardedTo]` 属性以非中断方式进行的。

#### <a name="recommended-action"></a>建议操作

- 引用 `Microsoft.AspNetCore.Server.Kestrel.Https` 2.0 的库应将所有 ASP.NET Core 依赖项更新为 2.1 或更高版本。 否则，它们可能会在加载到 ASP.NET Core 3.0 应用时中断。
- 面向 ASP.NET Core 2.1 和更高版本的应用和库应删除对 `Microsoft.AspNetCore.Server.Kestrel.Https` NuGet 包的任何直接引用。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
