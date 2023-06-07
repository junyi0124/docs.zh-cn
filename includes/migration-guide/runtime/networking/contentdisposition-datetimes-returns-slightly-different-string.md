---
ms.openlocfilehash: eb5c032a020799fa19cc0a8cfaabb56e01417ff4
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496387"
---
### <a name="contentdisposition-datetimes-returns-slightly-different-string"></a>ContentDisposition DateTimes 返回稍微不同的字符串

#### <a name="details"></a>详细信息

从 4.6 开始，已更新 <xref:System.Net.Mime.ContentDisposition?displayProperty=fullName> 的字符串表示形式，以始终使用两位数表示 <xref:System.DateTime?displayProperty=fullName> 的小时部分。 这是为了符合 [RFC822](https://www.ietf.org/rfc/rfc0822.txt) 和 [RFC2822](https://www.ietf.org/rfc/rfc2822.txt)。 这将导致在 4.6 中，当其中一个处理时间元素早于上午 10 点时，<xref:System.Net.Mime.ContentDisposition.ToString> 将返回稍微不同的字符串。 请注意，ContentDispositions 有时通过将它们转换为字符串以进行序列化，因此应检查任何 <xref:System.Net.Mime.ContentDisposition.ToString> 操作、序列化或 GetHashCode 调用。

#### <a name="suggestion"></a>建议

不要期望不同 .NET Framework 版本的 ContentDispositions 的字符串表示形式相互进行正确比较。 如果可以，请在执行比较前将字符串转换回 ContentDispositions。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.Mime.ContentDisposition.ToString?displayProperty=nameWithType>
- <xref:System.Net.Mime.ContentDisposition.GetHashCode?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Net.Mime.ContentDisposition.ToString`
- `M:System.Net.Mime.ContentDisposition.GetHashCode`

-->
