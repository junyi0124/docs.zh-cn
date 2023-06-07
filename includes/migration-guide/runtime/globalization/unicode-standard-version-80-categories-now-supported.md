---
ms.openlocfilehash: f389a9e9bcf4ac1777db66731a085d74889c4a98
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496740"
---
### <a name="unicode-standard-version-80-categories-now-supported"></a>现在支持 Unicode 标准版本 8.0 类别

#### <a name="details"></a>详细信息

在 .NET Framework 4.6.2 中，Unicode 数据已从 Unicode 标准版本 6.3 升级到版本 8.0。  在 .NET Framework 4.6.2 中请求 Unicode 字符类别时，某些结果可能与以前的 .NET Framework 版本中的结果不匹配。  此更改主要影响切罗基语音节和西双版纳新傣文元音标记和音调标记。

#### <a name="suggestion"></a>建议

检查代码并删除/更改依赖硬编码 Unicode 字符类别的逻辑。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Char.GetUnicodeCategory(System.Char)?displayProperty=nameWithType>
- <xref:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.Char)?displayProperty=nameWithType>
- <xref:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.String,System.Int32)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Char.GetUnicodeCategory(System.Char)`
- `M:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.Char)`
- `M:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.String,System.Int32)`

-->
