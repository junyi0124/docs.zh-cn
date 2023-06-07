---
ms.openlocfilehash: 5ad9c494fd02059e05cc744aad3b06cfc9399995
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "77451873"
---
### <a name="default-value-of-httprequestmessageversion-changed-to-11"></a>HttpRequestMessage.Version 的默认值已更改为 1.1

<xref:System.Net.Http.HttpRequestMessage.Version?displayProperty=fullName> 属性的默认值已从 2.0 更改为 1.1。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="change-description"></a>更改描述

在 .NET Core 1.0 至 2.0 中，<xref:System.Net.Http.HttpRequestMessage.Version?displayProperty=fullName> 属性的默认值为 1.1。 从 .NET Core 2.1 开始，该值已更改为 2.1。

从 .NET Core 3.0 开始，<xref:System.Net.Http.HttpRequestMessage.Version?displayProperty=fullName> 属性返回的默认版本号再次为 1.1。

#### <a name="recommended-action"></a>建议操作

如果代码依赖于 <xref:System.Net.Http.HttpRequestMessage.Version?displayProperty=fullName> 属性，则返回默认值 2.0，以更新代码。

#### <a name="category"></a>类别

网络

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.Http.HttpRequestMessage.Version?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Net.Http.HttpRequestMessage.Version`

-->
