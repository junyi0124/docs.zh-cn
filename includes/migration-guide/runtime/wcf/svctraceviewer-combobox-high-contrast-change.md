---
ms.openlocfilehash: 4bc8db52efdfe483acb4f6b6e33c4fa7716e0b79
ms.sourcegitcommit: 261e0c98a111357692b3b63c596edf0cacf72991
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90770826"
---
### <a name="svctraceviewer-combobox-high-contrast-change"></a>svcTraceViewer ComboBox 对比度更改

#### <a name="details"></a>详细信息

在 [Microsoft Service Trace Viewer 工具](~/docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md)中，在某些高对比度主题中，ComboBox 控件未以正确的颜色显示。 此问题已在 .NET Framework 4.7.2 中解决。 但是，由于 .NET Framework SDK 后向兼容性要求，因此默认情况下客户无法看到该修复程序。 .NET 4.8 通过将以下 [AppContext 配置开关](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)添加到 svcTraceViewer.exe.config 文件来表示此更改：

```xml
<AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
```

#### <a name="suggestion"></a>建议

如果不希望更改高对比度行为，可通过从 svcTraceViewer.exe.config 文件中删除以下部分来禁用它：

```xml
<AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
```

| “属性”    | “值”   |
|:--------|:--------|
| 范围   | 边缘    |
| Version | 4.8     |
| 类型    | 运行时 |

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
