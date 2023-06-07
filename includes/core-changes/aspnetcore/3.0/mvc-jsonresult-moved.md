---
ms.openlocfilehash: 96c2a32dd7cca91e965601d715bbd4625bba439a
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032297"
---
### <a name="mvc-jsonresult-moved-to-microsoftaspnetcoremvccore"></a>MVC：JsonResult 已移至 Microsoft.AspNetCore.Mvc.Core

`JsonResult` 已移至 `Microsoft.AspNetCore.Mvc.Core` 程序集。 此类型用于在 [Microsoft.AspNetCore.Mvc.Formatters.Json](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Json) 中进行定义。 已将程序集级别 [[TypeForwardedTo]](xref:System.Runtime.CompilerServices.TypeForwardedToAttribute) 属性添加到 `Microsoft.AspNetCore.Mvc.Formatters.Json`，以便为大多数用户解决此问题。 使用第三方库的应用可能会遇到问题。

#### <a name="version-introduced"></a>引入的版本

3.0 预览版 6

#### <a name="old-behavior"></a>旧行为

已成功生成使用基于 2.2 的库的应用。

#### <a name="new-behavior"></a>新行为

使用基于 2.2 的库的应用无法进行编译。 提供了包含以下文本变体的错误：

```output
The type 'JsonResult' exists in both 'Microsoft.AspNetCore.Mvc.Core, Version=3.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60' and 'Microsoft.AspNetCore.Mvc.Formatters.Json, Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'
```

有关此类问题的示例，请参阅 [dotnet/aspnetcore#7220](https://github.com/dotnet/aspnetcore/issues/7220)。

#### <a name="reason-for-change"></a>更改原因

按 [aspnet/Announcements#325](https://github.com/aspnet/Announcements/issues/325) 所述，对 ASP.NET Core 组成进行平台级别的更改。

#### <a name="recommended-action"></a>建议操作

根据 2.2 版本的 `Microsoft.AspNetCore.Mvc.Formatters.Json` 编译的库可能需要重新编译才能解决所有使用者的问题。 如果受到影响，请与库作者联系。 请求重新编译库以面向 ASP.NET Core 3.0。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Mvc.JsonResult?displayProperty=nameWithType>

<!-- 

#### Affected APIs

`T:Microsoft.AspNetCore.Mvc.JsonResult`

-->
