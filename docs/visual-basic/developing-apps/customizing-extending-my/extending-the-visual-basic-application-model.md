---
title: 扩展 Visual Basic 应用程序模型
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic Application Model, extending
ms.assetid: e91d3bed-4c27-40e3-871d-2be17467c72c
ms.openlocfilehash: e707f034f05aababdc70d5d6e1f9e1da0ed558bc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410226"
---
# <a name="extending-the-visual-basic-application-model"></a>扩展 Visual Basic 应用程序模型

通过重写 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> 类的 `Overridable` 成员，可以向应用程序模型添加功能。 借助此技术，可以在应用程序启动和关闭时自定义应用程序模型的行为，并添加对自己的方法的调用。

## <a name="visual-overview-of-the-application-model"></a>应用程序模型的视觉概述

此部分从视觉上呈现 Visual Basic 应用程序模型的函数调用序列。 下一个部分详细说明了各个函数的用途。

下图说明普通 Visual Basic Windows 窗体应用程序的应用程序模型调用序列。 `Sub Main` 过程调用 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> 方法时，此序列将启动。

![说明应用程序模型调用序列的关系图。](./media/extending-the-visual-basic-application-model/application-model-call-sequence.gif)

Visual Basic 应用程序模型还提供 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> 和 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> 事件。 下图说明用于引发这些事件的机制。

![说明引发 StartupNextInstance 事件的 OnStartupNextInstance 方法的关系图。](./media/extending-the-visual-basic-application-model/raise-startupnextinstance-event.gif)

![显示引发 UnhandledException 事件的 OnUnhandledException 方法的关系图。](./media/extending-the-visual-basic-application-model/raise-unhandledexception-event.gif)

## <a name="overriding-the-base-methods"></a>重写基方法

<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> 方法定义 `Application` 方法的运行顺序。 默认情况下，Windows 窗体应用程序的 `Sub Main` 过程调用 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> 方法。

若此应用程序是普通的应用程序（多实例应用程序）或单实例应用程序的第一个实例，<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> 方法按以下顺序执行 `Overridable` 方法：

1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A>。 默认情况下，此方法设置主应用程序线程的视觉样式、文本显示样式和当前主体（若此应用程序使用 Windows 身份验证），并调用 `ShowSplashScreen`（若 `/nosplash` 和 `-nosplash` 均未被用作命令行参数）。

     若此函数返回 `False`，将取消应用程序启动序列。 在某些情况下不应该运行此应用程序时，这会很有用。

     <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A> 方法调用以下方法：

    1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShowSplashScreen%2A>。 确定应用程序是否定义了初始屏幕，如果已定义，将在单独的线程中显示初始屏幕。

         <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShowSplashScreen%2A> 方法包含显示初始屏幕至少达 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MinimumSplashScreenDisplayTime%2A> 属性指定的毫秒数的代码。 若要使用此功能，必须使用项目设计器（它会将 `My.Application.MinimumSplashScreenDisplayTime` 属性设置为 2 秒）将初始屏幕添加到应用程序，或在重写 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A> 或 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A> 方法的方法中设置 `My.Application.MinimumSplashScreenDisplayTime` 属性。  有关详细信息，请参阅 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MinimumSplashScreenDisplayTime%2A>。

    2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A>。 允许设计器发出初始化初始屏幕的代码。

         默认情况下，此方法不执行任何操作。 若你从 Visual Basic 项目设计器为应用程序选择初始屏幕，设计器会使用将 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.SplashScreen%2A> 属性设置为初始屏幕窗体新实例的方法重写 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A> 方法。 

2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartup%2A>。 提供用于引发 `Startup` 事件的扩展点。 若此函数返回 `False`，应用程序启动序列会停止。

     默认情况下，此方法引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> 事件。 若事件处理程序将事件参数的 <xref:System.ComponentModel.CancelEventArgs.Cancel> 属性设置为 `True`，此方法会返回 `False`，用于取消应用程序启动。

3. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnRun%2A>。 在初始化完成后，提供主应用程序准备好开始运行时可运行的起点。

     默认情况下，在它进入 Windows 窗体消息循环之前，此方法调用 `OnCreateMainForm`（用于创建应用程序的主窗体）和 `HideSplashScreen`（用于关闭初始屏幕）方法：

    1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateMainForm%2A>。 为设计器发出初始化主窗体提供方法。

         默认情况下，此方法不执行任何操作。 不过，从 Visual Basic 项目设计器为应用程序选择主窗体时，设计器会使用将 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MainForm%2A> 属性设置为主窗体新实例的方法重写 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateMainForm%2A> 方法。 

    2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.HideSplashScreen%2A>。 若应用程序已定义初始屏幕并且它处于打开状态，此方法会关闭该初始屏幕。

         默认情况下，此方法会关闭初始屏幕。

4. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance%2A>。 提供一种自定义方法，自定义单实例应用程序在它的其他实例启动时的行为方式。

     默认情况下，此方法引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> 事件。

5. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnShutdown%2A>。 提供用于引发 `Shutdown` 事件的扩展点。 若主应用程序中出现未处理的异常，此方法将不会运行。

     默认情况下，此方法引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> 事件。

6. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnUnhandledException%2A>。 上面列出的任何方法中出现未处理的异常时，将执行它。

     默认情况下，只要未附加调试器并且应用程序正在处理 `UnhandledException` 事件，此方法就会引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> 事件。

 若应用程序是单实例应用程序，并且应用程序已在运行，此应用程序的后续实例会在应用程序的原始实例上调用 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance%2A> 方法，然后退出。

 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance(Microsoft.VisualBasic.ApplicationServices.StartupNextInstanceEventArgs)> 构造函数调用 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> 属性，来确定将哪个文本呈现引擎用于应用程序的窗体。 默认情况下，<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> 属性返回 `False`，来指示使用 GDI 文本呈现引擎，这是 Visual Basic 2005 以及更高版本中的默认设置。 可以将 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> 属性重写为返回 `True`，来指示使用 GDI+ 文本呈现引擎，这是 Visual Basic .NET 2002 和 Visual Basic .NET 2003 中的默认设置。

## <a name="configuring-the-application"></a>配置应用程序

 作为 Visual Basic 应用程序模型的一部分，<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> 类提供用于配置应用程序的受保护属性。 应在实现类的构造函数中设置这些属性。

 在默认的 Windows 窗体项目中，项目设计器生成代码来通过设计器设置设置属性。  仅在应用程序启动时使用这些属性；在应用程序启动后设置它们不起作用。

|Property|确定|项目设计器的“应用程序”窗格中的设置|
|---|---|---|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.IsSingleInstance%2A>|应用程序作为单实例还是作为多实例应用程序运行。|“生成单个实例应用程序”复选框 |
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.EnableVisualStyles%2A>|应用程序是否使用与 Windows XP 匹配的视觉样式。|“启用 XP 视觉样式”复选框 |
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.SaveMySettingsOnExit%2A>|应用程序退出时，应用程序是否自动保存应用程序的用户设置更改。|“关闭时保存 My.Settings”复选框 |
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShutdownStyle%2A>|何时终止应用程序，例如启动窗体关闭或最后一个窗体关闭时。|“关闭模式”列表 |

## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>
- [Visual Basic 应用程序模型概述](../development-with-my/overview-of-the-visual-basic-application-model.md)
- [“项目设计器”->“应用程序”页 (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
