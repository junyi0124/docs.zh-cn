---
ms.openlocfilehash: d420be76645fc71ac922542fa49f799a473e9a83
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614391"
---
### <a name="accessibility-improvements-in-windows-workflow-foundation-wf-workflow-designer"></a>Windows Workflow Foundation (WF) 工作流设计器中的辅助功能改进

#### <a name="details"></a>详细信息

Windows Workflow Foundation (WF) 工作流设计器正在改进辅助功能技术的工作原理。 这些改进包括以下更改：

- 某些控件中 Tab 键顺序更改为从左到右以及从上到下：
- 设置 <xref:System.ServiceModel.Activities.InitializeCorrelation> 活动相关数据的初始化相关窗口
- <xref:System.ServiceModel.Activities.Receive>、<xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply> 和 <xref:System.ServiceModel.Activities.ReceiveReply> 活动的内容定义窗口
- 通过键盘可以使用更多功能：
- 编辑活动的属性时，属性组在第一次聚焦时可以通过键盘折叠。
- 警告图标现在可以通过键盘访问。
- “属性”窗口的“更多属性”按钮现在可以通过键盘访问。
- 键盘用户现在可以访问工作流设计器“自变量和变量”窗格的标题项。
- 提升了聚焦项的可见性，例如当：
- 将行添加到工作流设计器和活动设计器使用的数据网格。
- 在 <xref:System.ServiceModel.Activities.ReceiveReply> 和 <xref:System.ServiceModel.Activities.SendReply> 活动中按 Tab 键切换字段。
- 设置变量或自变量的默认值
- 屏幕读取器现在可以正确识别：
- 工作流设计器中设置的断点。
- <xref:System.Activities.Statements.FlowSwitch%601>、<xref:System.Activities.Statements.FlowDecision> 和 <xref:System.ServiceModel.Activities.CorrelationScope> 活动。
- <xref:System.ServiceModel.Activities.Receive> 活动的内容。
- <xref:System.Activities.Statements.InvokeMethod> 活动的目标类型。
- <xref:System.Activities.Statements.TryCatch> 活动中的“异常”组合框和“最终”部分。
- 消息传递活动（<xref:System.ServiceModel.Activities.Receive>、<xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply> 和 <xref:System.ServiceModel.Activities.ReceiveReply>）中的“消息类型”组合框、“添加相关初始化表达式”窗口中的拆分器、“内容定义”窗口和“CorrelatesOn 定义”窗口。
- 状态机转换和转换目标。
- <xref:System.Activities.Statements.FlowDecision> 活动上的注释和连接器。
- 活动的上下文（右键单击）菜单。
- 属性值编辑器、“清除搜索”按钮、“按类别”和“按字母顺序”排序按钮以及属性网格中的“表达式编辑器”对话框。
- 工作流设计器中的缩放百分比。
- <xref:System.Activities.Statements.Parallel> 和 <xref:System.Activities.Statements.Pick> 活动中的分隔符。
- <xref:System.Activities.Statements.InvokeDelegate> 活动。
- 字典活动（`Microsoft.Activities.AddToDictionary&lt;TKey,TValue>`、`Microsoft.Activities.RemoveFromDictionary&lt;TKey,TValue>` 等）的“选择类型”窗口。
- “浏览和选择 .NET 类型”窗口。
- 工作流设计器中的痕迹导航。
- 选择高对比度主题的用户将看到工作流设计器及其控件可见性的许多改进，例如元素间更好的对比度和焦点元素更明显的选择框。

#### <a name="suggestion"></a>建议

如果你的应用程序有重新承载的工作流设计器，那么该应用程序可以通过执行其中任一操作从这些更改中受益：

- 重新编译应用程序以面向 .NET Framework 4.7.1。 这些辅助功能更改在默认情况下启用。
- 如果应用程序面向 .NET Framework 4.7 或更早版本，但在 .NET Framework 4.7.1 上运行，则可以通过将以下 [AppContext switch](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 添加到 app.config 文件的 `<runtime>` 部分并将其设为 `false`（如以下示例所示），选择弃用这些旧版辅助功能行为。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
  </runtime>
</configuration>
```

面向 .NET Framework 4.7.1 或更高版本并希望保留旧版辅助功能行为的应用程序，可通过将此 AppContext 开关显式设置为 `true` 来选择启用旧版辅助功能。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |
