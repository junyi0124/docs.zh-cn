---
title: Windows 窗体应用程序基础知识
ms.date: 07/20/2015
helpviewer_keywords:
- Windows applications
- Windows Forms, Visual Basic
ms.assetid: 0b919d30-7fd6-42db-85c8-543d15312441
ms.openlocfilehash: ef02662a5492f117b252e464e92ec46e7484ef66
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282168"
---
# <a name="windows-forms-application-basics-visual-basic"></a>Windows 窗体应用程序基础知识 (Visual Basic)

Visual Basic 的重要功能之一是能够创建在用户计算机上本地运行的 Windows 窗体应用程序。 可以使用 Visual Studio 通过 Windows 窗体创建应用程序和用户界面。 Windows 窗体应用程序基于 <xref:System.Windows.Forms> 命名空间中的类而构建。

## <a name="designing-windows-forms-applications"></a>设计 Windows 窗体应用程序

可以通过 Visual Studio 创建 Windows 窗体应用程序和 Windows 服务应用程序。 有关详细信息，请参阅下列主题：

- [Windows 窗体入门](/dotnet/desktop/winforms/getting-started-with-windows-forms) - 提供有关如何创建 Windows 窗体并对其编程的信息。

- [Windows 窗体控件](/dotnet/desktop/winforms/controls/) - 详细介绍如何使用 Windows 窗体控件的一系列主题。

- [Windows 服务应用程序](../../../framework/windows-services/index.md) - 列出了解释如何创建 Windows 服务的主题。

## <a name="building-rich-interactive-user-interfaces"></a>构建丰富的交互式用户界面

Windows 窗体是 .NET Framework 和 .NET Core（不低于 .NET Core 3.0）的智能客户端组件。 它是一组托管库，支持常见的应用程序任务，如对文件系统的读写。 使用类似于 Visual Studio 的开发环境时，可以创建 Windows 窗体应用程序，该应用程序可显示信息、请求来自用户的输入以及通过网络与远程计算机通信。

在 Windows 窗体中，窗体是一种可视图面，可在其上向用户显示信息。 通常情况下，可以通过在窗体上放置控件和开发对用户操作（如点击鼠标或按键）的响应来构建 Windows 窗体应用程序。 控件是离散的用户界面 (UI) 元素，用于显示数据或接受数据输入。

### <a name="events"></a>事件

当用户对你的窗体或一个窗体控件执行了某个操作时，该操作将生成一个事件。 你的应用程序通过使用代码对这些事件做出反应，并在事件发生时对其进行处理。 有关详细信息，请参阅[在 Windows 窗体中创建事件处理程序](/dotnet/desktop/winforms/creating-event-handlers-in-windows-forms)。

### <a name="controls"></a>控件

Windows 窗体包含各种可以放置在窗体上的控件：显示文本框、按钮、下拉框、单选按钮甚至网页的控件。 有关可在窗体上使用的所有控件的列表，请参阅[在 Windows 窗体上使用的控件](/dotnet/desktop/winforms/controls/controls-to-use-on-windows-forms)。 如果某个现有控件不满足你的需要，Windows 窗体还支持使用 <xref:System.Windows.Forms.UserControl> 类创建自己的自定义控件。

Windows 窗体具有丰富的 UI 控件，这些控件可模拟 Microsoft Office 等高端应用程序中的功能。 使用 <xref:System.Windows.Forms.ToolStrip> 和 <xref:System.Windows.Forms.MenuStrip> 控件时，可以创建包含文本和图像的工具栏和菜单、显示子菜单和托管其他控件（如文本框和组合框）。

利用 Visual Studio 拖放窗体设计器，可以轻松创建 Windows 窗体应用程序：只需使用光标选择控件，然后将它们放置在窗体上的目标位置即可。 设计器提供诸如网格线和对齐线的工具，以便简化对齐控件的操作。 无论使用 Visual Studio 还是在命令行进行编译，都可以使用 <xref:System.Windows.Forms.FlowLayoutPanel>、<xref:System.Windows.Forms.TableLayoutPanel> 和 <xref:System.Windows.Forms.SplitContainer> 控件花最少的时间和精力创建高级窗体布局。

### <a name="custom-ui-elements"></a>自定义 UI 元素

最后，如果必须创建自己的自定义 UI 元素，<xref:System.Drawing> 命名空间包含各种所需的类，可用以直接在窗体上呈现线条、圆形和其他形状。

有关如何使用这些功能的步骤信息，请参阅以下“帮助”主题。

|功能|查看|
|--------|---------|
|使用 Visual Studio 创建新的 Windows 窗体应用程序|[教程 1：创建图片查看器](/visualstudio/ide/tutorial-1-create-a-picture-viewer)|
|在窗体上使用控件|[如何：向 Windows 窗体添加控件](/dotnet/desktop/winforms/controls/how-to-add-controls-to-windows-forms)|
|使用 <xref:System.Drawing> 创建图形|[图形编程入门](/dotnet/desktop/winforms/advanced/getting-started-with-graphics-programming)|
|创建自定义控件|[如何：从 UserControl 类继承](/dotnet/desktop/winforms/controls/how-to-inherit-from-the-usercontrol-class)|

## <a name="displaying-and-manipulating-data"></a>显示和操作数据

许多应用程序必须显示数据库、XML 文件、XML Web 服务或其他数据源中的数据。 Windows 窗体提供了一个灵活的控件（名为 <xref:System.Windows.Forms.DataGridView> 控件），用于以传统的行和列格式呈现此类表格数据，以便使每段数据块都具有自己的单元格。 使用 <xref:System.Windows.Forms.DataGridView> 时，你可以自定义各个单元格的外观、将任意行和列锁定在所需位置、在单元格内部显示复杂控件，并可以使用其他功能。

通过网络连接到数据源对于 Windows 窗体智能客户端而言是一个简单的任务。 <xref:System.Windows.Forms.BindingSource> 组件（对于 Visual Studio 2005 和 .NET Framework 2.0 中的 Windows 窗体而言是全新内容）表示与数据源的连接，可以公开将数据绑定到控件的方法、导航到上一个和下一个记录、编辑记录并且将更改保存到原始源。 <xref:System.Windows.Forms.BindingNavigator> 控件通过 <xref:System.Windows.Forms.BindingSource> 组件提供一个简单界面，可供用户在记录间导航。

### <a name="data-bound-controls"></a>数据绑定控件

“数据源”窗口显示项目中的数据库、Web 服务和对象，你可以使用它来轻松创建数据绑定控件。 可以通过将此窗口中的项拖动到项目中的窗体上来创建数据绑定控件。 还可以通过将对象从“数据源”窗口拖动到现有控件上来将现有控件与数据进行数据绑定。

### <a name="settings"></a>设置

可在 Windows 窗体中管理的另一类数据绑定是“设置”。 大多数智能客户端应用程序均必须保留有关其运行时状态的某些信息（例如窗体的最后已知大小）并保留用户首选项数据（例如保存的文件的默认位置）。 “应用程序设置”功能通过提供一种在客户端计算机上存储两种类型的设置的简单方法来满足这些要求。 使用 Visual Studio 或代码编辑器定义这些设置后，这些设置便作为 XML保留并自动在运行时读取回内存中。

有关如何使用这些功能的步骤信息，请参阅以下“帮助”主题。

|功能|查看|
|--------|---------|
|使用 <xref:System.Windows.Forms.BindingSource> 组件|[如何：使用设计器将 Windows 窗体控件与 BindingSource 组件进行绑定](/dotnet/desktop/winforms/controls/bind-wf-controls-with-the-bindingsource)|
|使用 ADO.NET 数据源|[如何：使用 Windows 窗体 BindingSource 组件对 ADO.NET 数据进行排序和筛选](/dotnet/desktop/winforms/controls/sort-and-filter-ado-net-data-with-wf-bindingsource-component)|
|使用“数据源”窗口|[演练：在 Windows 窗体上显示数据](/visualstudio/data-tools/accessing-data-in-visual-studio)|

## <a name="deploying-applications-to-client-computers"></a>将应用程序部署到客户端计算机

编写应用程序后，必须将应用程序发送给用户，以便他们可以在自己的客户端计算机上安装和运行此应用程序。 使用 ClickOnce 技术时，只需进行几次点击便可以从 Visual Studio 内部署应用程序，然后向用户提供指向 Web 上的应用程序的 URL。 ClickOnce 管理你的应用程序中的所有元素和依赖项，并确保应用程序正确安装到客户端计算机上。

ClickOnce 应用程序可以配置为仅在用户连接到网络时运行，或配置为联机和脱机时均可运行。 如果指定应用程序应支持脱机操作，则 ClickOnce 会在用户的“开始”菜单中添加一个指向应用程序的链接，以便用户无需使用 URL 即可打开应用程序。

更新应用程序时，便向你的 Web 服务器发布了一个新的部署清单和应用程序的一个新副本。 ClickOnce 将检测到有可用更新并将升级用户的安装；更新旧程序集无需进行自定义编程。

有关 ClickOnce 的完整介绍，请参阅 [ClickOnce 安全和部署](/visualstudio/deployment/clickonce-security-and-deployment)。 有关如何使用这些功能的步骤信息，请参阅以下“帮助”主题：

|功能|查看|
|--------|---------|
|使用 ClickOnce 部署应用程序|[如何：使用发布向导发布 ClickOnce 应用程序](/visualstudio/deployment/how-to-publish-a-clickonce-application-using-the-publish-wizard)<br /><br /> [演练：手动部署 ClickOnce 应用程序](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)|
|更新 ClickOnce 部署|[如何：管理 ClickOnce 应用程序的更新](/visualstudio/deployment/how-to-manage-updates-for-a-clickonce-application)|
|使用 ClickOnce 管理安全性|[如何：启用 ClickOnce 安全设置](/visualstudio/deployment/how-to-enable-clickonce-security-settings)|

## <a name="other-controls-and-features"></a>其他控件和功能

Windows 窗体中还有许多其他功能，有助于轻松、快速地实现常见任务，如支持创建对话框、打印、添加文档以及将应用程序本地化为多种语言。 此外，Windows 窗体还依赖 .NET 的可靠安全系统，这样你就能向客户发布更安全的应用程序。

有关如何使用这些功能的步骤信息，请参阅以下“帮助”主题：

|功能|查看|
|--------|---------|
|打印窗体内容|[如何：打印 Windows 窗体中的图形](/dotnet/desktop/winforms/advanced/how-to-print-graphics-in-windows-forms)<br /><br /> [如何：打印 Windows 窗体中的多页文本文件](/dotnet/desktop/winforms/advanced/how-to-print-a-multi-page-text-file-in-windows-forms)|
|了解有关 Windows 窗体安全的详细信息|[Windows 窗体中的安全性概述](/dotnet/desktop/winforms/security-in-windows-forms-overview)|

## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- [Windows 窗体概述](/dotnet/desktop/winforms/windows-forms-overview)
- [My.Forms 对象](../../language-reference/objects/my-forms-object.md)
