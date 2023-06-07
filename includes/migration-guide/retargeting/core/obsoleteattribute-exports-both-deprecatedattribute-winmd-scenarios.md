---
ms.openlocfilehash: 7d7424b3ca0d295022999e95ed9862b5226b6220
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606339"
---
### <a name="obsoleteattribute-exports-as-both-obsoleteattribute-and-deprecatedattribute-in-winmd-scenarios"></a>ObsoleteAttribute 在 WinMD 方案中导出为 ObsoleteAttribute 和 DeprecatedAttribute

#### <a name="details"></a>详细信息

创建 Windows 元数据库（.winmd 文件）时，<xref:System.ObsoleteAttribute?displayProperty=fullName> 属性将导出为 <xref:System.ObsoleteAttribute?displayProperty=fullName> 和 [Windows.Foundation.DeprecatedAttribute](/uwp/api/windows.foundation.metadata.deprecatedattribute)。

#### <a name="suggestion"></a>建议

重新编译使用 <xref:System.ObsoleteAttribute?displayProperty=fullName> 属性的现有源代码可能会在从 C++/CX 或 JavaScript 使用该代码时生成警告。我们不建议将 <xref:System.ObsoleteAttribute?displayProperty=fullName> 和 [Windows.Foundation.DeprecatedAttribute](/uwp/api/windows.foundation.metadata.deprecatedattribute) 同时用于托管程序集中的代码，这可能会导致生成警告。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.5.1       |
| 类型    | 重定目标 |
