---
title: XAML 中的 WPF 和 WF 集成
ms.date: 03/30/2017
ms.assetid: a4f53b48-fc90-4315-bca0-ba009562f488
ms.openlocfilehash: b8015e5c526f58875beb58ab48a21c050bdc8a06
ms.sourcegitcommit: 42ed59871db1f29a32b3d8e7abeb20e6eceeda7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2019
ms.locfileid: "74960085"
---
# <a name="wpf-and-windows-workflow-foundation-integration-in-xaml"></a>XAML 中的 WPF 和 Windows Workflow Foundation 集成

此示例演示如何创建一个应用程序，该应用程序在一个 XAML 文档中使用 Windows Presentation Foundation （WPF）和 Windows Workflow Foundation （WF）功能。 为实现此目的，此示例使用 Windows Workflow Foundation 和 XAML 扩展性。

## <a name="sample-details"></a>示例详细信息

 ShowWindow.xaml 文件反序列化为一个具有两个字符串变量的 <xref:System.Activities.Statements.Sequence> 活动，这两个变量由序列的 `ShowWindow` 和 `WriteLine` 活动操作。 <xref:System.Activities.Statements.WriteLine> 活动将它分配给 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性中的表达式输出到控制台窗口。 `ShowWindow` 活动显示 WPF 窗口作为其执行逻辑的一部分。 窗口的 <xref:System.Activities.ActivityContext.DataContext%2A> 包含在序列中声明的变量。 在 `ShowWindow` 活动中声明的窗口控件使用数据来操作这些变量。 最后，窗口将包含一个按钮控件。 按钮的 `Click` 事件由名为 <xref:System.Activities.ActivityDelegate> 的 `MarkupExtension` 处理，它包含 `CloseWindow` 活动。 `MarkUpExtension` 将调用包含的这个活动，用于提供由 `x:Name` 和包含窗口的 <xref:System.Activities.ActivityContext.DataContext%2A> 所标识的任意对象（作为上下文）。 因此，可以使用一个引用窗口名称的表达式来绑定 `CloseWindow.InArgument<Window>`。

 `ShowWindow` 活动派生自 <xref:System.Activities.AsyncCodeActivity%601> 类以显示 WPF 窗口，并在窗口关闭时完成。 `Window` 属性的类型为 `Func<Window>`，该类型允许在每次执行活动时按需创建窗口。 `Window` 属性使用一个 <xref:System.Xaml.XamlDeferringLoader> 来启用此延迟计算模型。 `FuncFactoryDeferringLoader` 允许在序列化期间捕获一个 `XamlReader`，然后在活动执行期间读取它。

 正确编写的活动从不会阻塞计划程序线程。 然而，`ShowWindow` 活动只有在它显示的窗口关闭之后才能完成。 `ShowWindow` 活动通过以下方式来实现此行为：从 <xref:System.Activities.AsyncCodeActivity> 派生，在 <xref:System.Activities.WorkflowInvoker.BeginInvoke%2A> 方法中调用 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 方法，然后显示有模式窗口。 将通过 WPF <xref:System.ServiceModel.InstanceContext.SynchronizationContext%2A> 调用此委托。 `ShowWindow` 活动将 <xref:System.Activities.ActivityContext.DataContext%2A> 属性分配给 `Window.DataContext` 属性，以便任何数据绑定控件能够访问范围内变量。

 此示例中需要注意的最后一个内容是名为 <xref:System.Workflow.ComponentModel.Serialization.MarkupExtension> 的 `DelegateActivityExtension`。 此标记扩展 `ProvideValue` 的方法将返回一个调用嵌入活动的委托。 此活动在包含 WPF 数据上下文和范围中的任何 `x:Name` 值的环境中运行。 在 `GenericInvoke` 方法中，可通过一个 <xref:System.Activities.Hosting.SymbolResolver> 扩展来将此环境提供给相应的活动。 将此扩展添加到 <xref:System.Activities.WorkflowInvoker> 中，然后在调用标记扩展的委托时，使用后者来调用嵌入的活动。

> [!NOTE]
> 默认设计器不支持 ShowWindow 活动；同样，ShowWindow.Xaml 文件无法在设计器上正确显示。

## <a name="run-the-sample"></a>运行示例

1. 使用 Visual Studio 打开 WPFWFIntegration 解决方案文件。

2. 若要生成解决方案，请按**Ctrl**+**Shift**+**B**。

3. 若要运行解决方案，请按**F5**。

4. 在对话框中键入您的名字和姓氏。

5. 关闭对话框，控制台将回显您的姓名。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\WPFWFIntegration`
