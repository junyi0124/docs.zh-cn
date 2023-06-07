---
ms.openlocfilehash: 418bcdca1e9a325894891d7b0e080ce035e2d1b4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614338"
---
### <a name="aspnet-accessibility-improvements-in-net-framework-471"></a>.NET Framework 4.7.1 中的 ASP.NET 辅助功能改进

#### <a name="details"></a>详细信息

从 .NET Framework 4.7.1 开始，ASP.NET 改进了 ASP.NET Web 控件与 Visual Studio 中的辅助功能技术配合使用的方式，以更好地支持 ASP.NET 客户。  其中包括以下更改：

- 在以下控件中实现缺失 UI 的辅助功能模式：例如“详细信息视图”向导中的“添加字段”对话框或“ListView”向导的“配置 ListView”对话框。
- 改善在高对比度模式下（如数据页导航字段编辑器）的显示。
- 更改用于改善以下控件的键盘导航体验：如 DataPager 控件的“编辑页导航字段”向导中的“字段”对话框、“配置 ObjectContext”对话框或“配置数据源”向导的“配置数据选择”对话框。

#### <a name="suggestion"></a>建议

**如何选择启用或弃用这些更改** 为使 Visual Studio 设计器从这些更改中获益，它必须在 .NET Framework 4.7.1 或更高版本上运行。 Web 应用程序可通过以下任何一种方式从这些更改中获益：

- 安装 Visual Studio 2017 15.3 或更高版本，它在默认情况下使用以下 AppContext 开关支持新的辅助功能。
- 如以下示例所示，通过将 `Switch.UseLegacyAccessibilityFeatures` AppContext 开关添加到 devenv.exe.config 文件中的 `<runtime>` 部分并将其设置为 `false`，可选择弃用旧版辅助功能行为。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
<runtime>
...
<!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false'  -->
<AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
...
</runtime>
</configuration>
```

面向 .NET Framework 4.7.1 或更高版本并希望保留旧版辅助功能行为的应用程序，可通过将此 AppContext 开关显式设置为 `true` 来选择启用旧版辅助功能。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |
