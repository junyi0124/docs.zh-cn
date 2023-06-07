---
ms.openlocfilehash: 75945e7ff26c1c6db891d12cef4c16ed210da6af
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032300"
---
### <a name="mvc-web-api-compatibility-shim-removed"></a>MVC：已删除 Web API 兼容性填充码

从 ASP.NET Core 3.0 开始，不再提供 `Microsoft.AspNetCore.Mvc.WebApiCompatShim` 包。

#### <a name="change-description"></a>更改描述

`Microsoft.AspNetCore.Mvc.WebApiCompatShim` (WebApiCompatShim) 包在 ASP.NET Core 中提供了与 ASP.NET 4.x Web API 2 的部分兼容性，以简化将现有 Web API 实现迁移到 ASP.NET Core 的工作。 但是，使用 WebApiCompatShim 的应用不会从最近 ASP.NET Core 版本中提供的与 API 相关的功能中获益。 此类功能包括改进的开放 API 规范生成、标准化错误处理和客户端代码生成。 为了更好地专注于 3.0 中的 API 工作，已删除 WebApiCompatShim。 使用 WebApiCompatShim 的现有应用应迁移到较新的 `[ApiController]` 模型。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="reason-for-change"></a>更改原因

Web API 兼容性填充码是一种迁移工具。 它限制用户对 ASP.NET Core 中添加的新功能的访问。

#### <a name="recommended-action"></a>建议操作

删除此填充码的使用，直接迁移到 ASP.NET Core 本身中的类似功能。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Mvc.WebApiCompatShim?displayProperty=fullName>

<!--

#### Affected APIs

N:Microsoft.AspNetCore.Mvc.WebApiCompatShim

-->
