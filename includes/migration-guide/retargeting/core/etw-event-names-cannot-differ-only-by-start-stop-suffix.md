---
ms.openlocfilehash: 0e25d5d9b545e5cb400cbf701fb13da572fadf10
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614335"
---
### <a name="etw-event-names-cannot-differ-only-by-a-start-or-stop-suffix"></a>ETW 事件名称无法仅通过“Start”或“Stop”后缀区别开来

#### <a name="details"></a>详细信息

在 .NET Framework 4.6 和 4.6.1 中，当两个 Windows 事件跟踪 (ETW) 事件名称只有“Start”或“Stop”后缀不同时（例如，当一个事件命名为 `LogUser`，另一个事件命名为 `LogUserStart`），运行时会引发 <xref:System.ArgumentException>。 在这种情况下，运行时不会构建不能发出任何日志记录的事件源。

#### <a name="suggestion"></a>建议

若要避免此异常，请确保两个事件名称不是只有“Start”或“Stop”后缀不同。从 .NET Framework 4.6.2 开始，将删除此要求，运行时可区分只有“Start”和“Stop”后缀不同的事件名称。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6         |
| 类型    | 重定目标 |
