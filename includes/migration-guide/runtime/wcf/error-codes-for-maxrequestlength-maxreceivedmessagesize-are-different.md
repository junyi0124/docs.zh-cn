---
ms.openlocfilehash: 26facb645de175d7ef0432394fc2b84f59e8437d
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497872"
---
### <a name="error-codes-for-maxrequestlength-or-maxreceivedmessagesize-are-different"></a>maxRequestLength 或 maxReceivedMessageSize 的错误代码是不同的

#### <a name="details"></a>详细信息

Internet Information Services (IIS) 或 ASP.NET 开发服务器承载的 WCF Web 服务中的消息如果超过 maxRequestLength（在 ASP.NET 中）或 maxReceivedMessageSize（在 WCF 中），则有不同的错误代码。HTTP 状态代码已从 400（错误请求）更改为 413（请求实体太大），并且超过 maxRequestLength 或 maxReceivedMessageSize 设置的消息引发了 <xref:System.ServiceModel.ProtocolException?displayProperty=fullName> 异常。 这包括转换模式为“流式处理”的情况。

#### <a name="suggestion"></a>建议

在消息长度超过 ASP.NET 或 WCF 所允许的限制的情况下，此更改有利于调试。必须基于 HTTP 400 状态代码修改执行处理的任何代码。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
