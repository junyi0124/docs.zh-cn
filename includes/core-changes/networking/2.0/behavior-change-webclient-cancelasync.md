---
ms.openlocfilehash: 80d4bbbfe52ab9685d7ca54f21b80d4b1773da22
ms.sourcegitcommit: d9c7ac5d06735a01c1fafe34efe9486734841a72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2020
ms.locfileid: "82859638"
---
### <a name="webclientcancelasync-doesnt-always-cancel-immediately"></a>WebClient.CancelAsync 并不总是立即取消

自 .NET Core 2.0 起，如果响应已开始提取，则调用 <xref:System.Net.WebClient.CancelAsync?displayProperty=nameWithType> 不会立即取消请求。

#### <a name="change-description"></a>更改描述

以前，调用 <xref:System.Net.WebClient.CancelAsync?displayProperty=nameWithType> 会立即取消请求。 自 .NET Core 2.0 起，只有当响应尚未开始提取时，调用 <xref:System.Net.WebClient.CancelAsync?displayProperty=nameWithType> 才会立即取消请求。 如果响应已开始提取，则只有在读取完整响应之后才会取消请求。

此更改已实现，因为 <xref:System.Net.WebClient> API 已弃用，取而代之的是 <xref:System.Net.Http.HttpClient>。

#### <a name="version-introduced"></a>引入的版本

2.0

#### <a name="recommended-action"></a>建议操作

使用 <xref:System.Net.Http.HttpClient?displayProperty=fullName> 类，而不是已弃用的 <xref:System.Net.WebClient?displayProperty=fullName>。

#### <a name="category"></a>类别

网络

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.WebClient.CancelAsync?displayProperty=fullName>

<!--

#### Affected APIs

- `M:System.Net.WebClient.CancelAsync`

-->
