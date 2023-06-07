---
ms.openlocfilehash: 31e7f84a787d255a474f4c2b1fa3068903dbed52
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032273"
---
### <a name="http-headernames-constants-changed-to-static-readonly"></a>HTTP：HeaderNames 常量已更改为静态只读

从 ASP.NET Core 3.0 预览版 5 开始，<xref:Microsoft.Net.Http.Headers.HeaderNames?displayProperty=fullName> 中的字段已从 `const` 更改为 `static readonly`。

有关讨论，请参阅 [dotnet/aspnetcore#9514](https://github.com/dotnet/aspnetcore/issues/9514)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

这些字段以前是 `const`。

#### <a name="new-behavior"></a>新行为

这些字段现在是 `static readonly`。

#### <a name="reason-for-change"></a>更改原因

更改：

* 防止将值嵌入到程序集边界内，允许根据需要更正值。
* 实现更快的引用同等性检查。

#### <a name="recommended-action"></a>建议操作

针对 3.0 重新编译。 通过以下方式使用这些字段的源代码将无法再执行此项操作：

* 作为特性参数
* 作为 `switch` 语句中的 `case`
* 定义其他 `const` 时

若要解决中断性变更，请切换到使用自定义标头名称常量或字符串文本。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.Net.Http.Headers.HeaderNames?displayProperty=fullName>

<!-- 

#### Affected APIs

`T:Microsoft.Net.Http.Headers.HeaderNames`

-->
