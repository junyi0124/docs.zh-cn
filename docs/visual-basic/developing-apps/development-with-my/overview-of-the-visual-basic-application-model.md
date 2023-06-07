---
title: Visual Basic 应用程序模型概述
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application object [Visual Basic], Visual Basic application model
- Visual Basic application model
ms.assetid: 17538984-84fe-43c9-82c8-724c9529fe8b
ms.openlocfilehash: 46177d0af7e5df767eb8421caf424880baac615e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411722"
---
# <a name="overview-of-the-visual-basic-application-model"></a>Visual Basic 应用程序模型概述

Visual Basic 提供定义完善的模型来控制 Windows 窗体应用程序的行为：Visual Basic 应用程序模型。 此模型包含用于处理应用程序的启动和关闭的事件，以及用于捕获未经处理的异常的事件。 它还为开发单实例应用程序提供支持。 该应用程序模型是可扩展的，因此需要更多控制的开发人员可以自定义可重写的方法。  
  
## <a name="uses-for-the-application-model"></a>应用程序模型的使用  

 典型应用程序在启动和关闭时需要执行任务。 例如在启动时，应用程序可以显示初始屏幕、进行数据库连接、加载已保存状态等。 在应用程序关闭时，它可以关闭数据库连接、保存当前状态等。 此外，应用程序可以在应用程序意外关闭时（例如在未经处理的异常期间）执行特定代码。  
  
 使用 Visual Basic 应用程序模型可轻松创建单实例  应用程序。 单实例应用程序与普通应用程序的不同之处在于，一次只能运行应用程序的一个实例。 尝试启动单实例应用程序的另一个实例会导致通过 `StartupNextInstance` 事件向原始实例通知进行了另一次启动尝试。 通知包含后续实例的命令行参数。 应用程序的后续实例随后会先关闭，然后才能进行任何初始化。  
  
 单实例应用程序会启动并检查它是应用程序的第一个实例还是后续实例：  
  
- 如果它是第一个实例，则它会照常启动。  
  
- 在第一个实例运行期间，启动应用程序的每个后续尝试都会导致非常不同的行为。 后续尝试会向第一个实例通知命令行参数，然后立即退出。 第一个实例会处理 `StartupNextInstance` 事件，以确定后续实例的命令行参数是什么并继续运行。  
  
     此图显示了后续实例如何向第一个实例发出信号：  
  
     ![显示单实例应用程序图像的关系图。](./media/overview-of-the-visual-basic-application-model/single-instance-application.gif)  
  
 通过处理 `StartupNextInstance` 事件，可以控制单实例应用程序的行为方式。 例如，Microsoft Outlook 通常作为单实例应用程序运行；当 Outlook 正在运行并且你尝试再次启动 Outlook 时，焦点将移动到原始实例上，但另一个实例不会打开。  
  
## <a name="events-in-the-application-model"></a>应用程序模型中的事件  

 应用程序模型中有以下事件：  
  
- 应用程序启动  。 应用程序在启动时会引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> 事件。 通过处理此事件，可以添加在加载主窗体之前初始化应用程序的代码。 如果需要，`Startup` 事件还可用于在启动过程的该阶段取消应用程序执行。  
  
     可以将应用程序配置为在应用程序启动代码运行时显示初始屏幕。 默认情况下，使用 `/nosplash` 或 `-nosplash` 命令行参数时，应用程序模型会禁止显示初始屏幕。  
  
- 单实例应用程序  。 当单实例应用程序的后续实例启动时，将引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> 事件。 该事件会传递后续实例的命令行参数。  
  
- 未经处理的异常  。 如果应用程序遇到未经处理的异常，则会引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> 事件。 该事件的处理程序可以检查异常，并确定是否要继续执行。  
  
     在某些情况下，不会引发 `UnhandledException` 事件。 有关详细信息，请参阅 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>。  
  
- 网络连接更改  。 如果计算机的网络可用性更改，则应用程序会引发 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged> 事件。  
  
     在某些情况下，不会引发 `NetworkAvailabilityChanged` 事件。 有关详细信息，请参阅 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>。  
  
- 应用程序关闭  。 应用程序提供 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> 事件，用于在即将关闭时发出信号。 在该事件处理程序中，可以确保应用程序需要执行的操作（例如，关闭和保存）已完成。 可以将应用程序配置为在主窗体关闭时关闭，或是仅当所有窗体都关闭时才关闭。  
  
## <a name="availability"></a>可用性  

 默认情况下，Visual Basic 应用程序模型可用于 Windows 窗体项目。 如果将应用程序配置为使用不同的启动对象，或使用自定义 `Sub Main` 启动应用程序代码，则该对象或类可能需要提供 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> 类的实现以使用应用程序模型。 有关更改启动对象的信息，请参阅[项目设计器的应用程序页 (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- [扩展 Visual Basic 应用程序模型](../customizing-extending-my/extending-the-visual-basic-application-model.md)
