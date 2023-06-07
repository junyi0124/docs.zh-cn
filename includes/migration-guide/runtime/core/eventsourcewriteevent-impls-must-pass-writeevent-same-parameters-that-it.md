---
ms.openlocfilehash: 99a7fa0fcfce6d490a182f85709b5dd0e0e8c86f
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496273"
---
### <a name="eventsourcewriteevent-impls-must-pass-writeevent-the-same-parameters-that-it-received-plus-id"></a>EventSource.WriteEvent impls 必须传递它接收的相同参数 WriteEvent（以及 ID）

#### <a name="details"></a>详细信息

运行时现在强制执行指定以下内容的合同：如果类派生自 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> 且用于定义 ETW 事件方法，它必须使用事件 ID（后跟已传递给 ETW 事件方法的相同参数）调用基类 <code>EventSource.WriteEvent</code> 方法。

#### <a name="suggestion"></a>建议

如果 <xref:System.IndexOutOfRangeException?displayProperty=fullName> 读取违反此协定的事件源进程中的 <xref:System.Diagnostics.Tracing.EventListener?displayProperty=fullName> 数据，则将引发 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> 异常。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
