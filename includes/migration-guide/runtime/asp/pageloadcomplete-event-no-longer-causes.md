---
ms.openlocfilehash: a84d72813a46d6bb39f4710b35d2c9dc859e30ef
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496310"
---
### <a name="pageloadcomplete-event-no-longer-causes-systemwebuiwebcontrolsentitydatasource-control-to-invoke-data-binding"></a>Page.LoadComplete 事件不再导致 System.Web.UI.WebControls.EntityDataSource 控件调用数据绑定

#### <a name="details"></a>详细信息

<xref:System.Web.UI.Page.LoadComplete> 事件不再导致 <xref:System.Web.UI.WebControls.EntityDataSource?displayProperty=fullName> 控件针对 create/update/delete 参数的更改来调用数据绑定。 此更改消除到数据库的外来行程，防止重置控件的值，并生成与其他数据控件一致的行为，例如 <xref:System.Web.UI.WebControls.SqlDataSource?displayProperty=fullName> 和 <xref:System.Web.UI.WebControls.ObjectDataSource?displayProperty=fullName>。 在应用程序依赖于在 <xref:System.Web.UI.Page.LoadComplete> 事件中调用数据绑定的极少数情况下，此更改会产生不同的行为。

#### <a name="suggestion"></a>建议

如果需要数据绑定，请在回发之前的事件中手动调用数据绑定。

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
