---
ms.openlocfilehash: e42bce91afab68e509cb35a8992fa3ca2f096872
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497939"
---
### <a name="wpf-printing-stack-update"></a>WPF 打印堆栈更新

#### <a name="details"></a>详细信息

WPF 打印 API 现在使用 <xref:System.Printing.PrintQueue?displayProperty=fullName> 调用窗口的打印文档包 API，以支持现已弃用的 XPS 打印 API。 这一更改是基于可维护性考虑的，因此用户和开发者应该都不会看到任何行为或 API 使用变化。 当在 Windows 10 创意者更新中运行时，此新打印堆栈默认情况下处于启用状态。 在旧版 Windows 中，旧打印堆栈将继续像过去一样运行。

#### <a name="suggestion"></a>建议

若要在 Windows 10 创意者更新中使用旧堆栈，请将 <code>HKEY_CURRENT_USER\Software\Microsoft\.NETFramework\Windows Presentation Foundation\Printing</code> 注册表项的 <code>UseXpsOMPrinting</code> REG_DWORD 值设置为 <code>1</code>。

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.7|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
