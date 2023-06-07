---
ms.openlocfilehash: f78d15338aa49de5b729aca12964924a0df00ec6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614386"
---
### <a name="improved-accessibility-for-some-net-sdk-tools"></a>某些 .NET SDK 工具的改进的辅助功能

#### <a name="details"></a>详细信息

在 .NET Framework SDK 4.7.1 中，已通过修复各种辅助功能问题，改进了 SvcConfigEditor.exe 和 SvcTraceViewer.exe 工具。 其中大多数都是一些小问题，如未定义名称或未正确实现某些 UI 自动化模式。 虽然许多用户不会意识到这些小问题的重要性，但使用屏幕阅读器等辅助技术的客户会发现这些 SDK 工具更易于访问。 当然，这些修复程序改变了以前的某些行为，如键盘焦点顺序。为获取这些工具中的所有辅助功能修复程序，可对 app.config 文件执行以下操作：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false"/>
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.1       |
| 类型    | 重定目标 |
