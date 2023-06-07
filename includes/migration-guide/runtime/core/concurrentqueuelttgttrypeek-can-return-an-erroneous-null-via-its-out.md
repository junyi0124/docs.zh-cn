---
ms.openlocfilehash: 004e2d1883b631e88ab5e164b1120c3b081b7041
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497576"
---
### <a name="concurrentqueuelttgttrypeek-can-return-an-erroneous-null-via-its-out-parameter"></a>ConcurrentQueue&lt;T&gt;.TryPeek 可通过 out 参数返回错误的 null

#### <a name="details"></a>详细信息

在某些多线程情况下，<xref:System.Collections.Concurrent.ConcurrentQueue%601.TryPeek(%600@)?displayProperty=fullName> 可返回 true，但使用 null 值填充 Out 参数（而非正确的扫视值）。

#### <a name="suggestion"></a>建议

此问题已在 .NET Framework 4.5.1 中解决。 升级到该 Framework 即可解决该问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |主要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Collections.Concurrent.ConcurrentQueue%601.TryPeek(%600@)?displayProperty=nameWithType>

<!--

#### Affected APIs

- ``M:System.Collections.Concurrent.ConcurrentQueue`1.TryPeek(`0@)``

-->
