---
ms.openlocfilehash: cd66317bc93343e03a73dec74dff534776ca42e4
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032275"
---
### <a name="http-response-body-infrastructure-changes"></a>HTTP：响应正文基础结构更改

支持 HTTP 响应正文的基础结构已发生更改。 如果直接使用 `HttpResponse`，则不需要进行任何代码更改。 如果要包装或替换 `HttpResponse.Body` 或访问 `HttpContext.Features`，请进行进一步了解。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

有三个 API 与 HTTP 响应正文关联：

- `IHttpResponseFeature.Body`
- `IHttpSendFileFeature.SendFileAsync`
- `IHttpBufferingFeature.DisableResponseBuffering`

#### <a name="new-behavior"></a>新行为

如果替换 `HttpResponse.Body`，则它通过使用 `StreamResponseBodyFeature` 为所有预期的 API 提供默认实现，将整个 `IHttpResponseBodyFeature` 替换为给定流周围的包装器。 重新设置回原始流会还原此更改。

#### <a name="reason-for-change"></a>更改原因

动机是将响应正文 API 合并为单一新功能接口。

#### <a name="recommended-action"></a>建议操作

使用之前在其中使用 `IHttpResponseFeature.Body`、`IHttpSendFileFeature` 或 `IHttpBufferingFeature` 的 `IHttpResponseBodyFeature`。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Http.Features.IHttpBufferingFeature?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature.Body?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `T:Microsoft.AspNetCore.Http.Features.IHttpBufferingFeature`
- `P:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature.Body`
- `T:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature`

-->
