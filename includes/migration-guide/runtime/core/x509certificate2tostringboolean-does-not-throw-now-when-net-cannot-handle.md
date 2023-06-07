---
ms.openlocfilehash: 7f8f97adcca7e66fa5756212bb977daadd92b8b6
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496852"
---
### <a name="x509certificate2tostringboolean-does-not-throw-now-when-net-cannot-handle-the-certificate"></a>现在，当 .NET 无法处理证书时，不会引发 X509Certificate2.ToString(Boolean)

#### <a name="details"></a>详细信息

在 .NET Framework 4.5.2 及更早版本中，如果为详细参数传递了 <code>true</code>，并且安装了 .NET Framework 不支持的证书，则会引发此方法。 现在，该方法将取得成功，并返回省略证书无法访问部分的有效字符串。

#### <a name="suggestion"></a>建议

应更新任何依赖 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.ToString(System.Boolean)?displayProperty=nameWithType> 的代码，以希望返回的字符串会排除在某些情况下，API 之前曾引发的某些证书数据（例如公钥、私钥和扩展）。

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.ToString(System.Boolean)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Security.Cryptography.X509Certificates.X509Certificate2.ToString(System.Boolean)`

-->
