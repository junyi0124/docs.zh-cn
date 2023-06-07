---
title: Windows 服务应用程序介绍
description: 查看 Windows 服务应用程序简介。 使用服务，你可以创建能在自身 Windows 会话中长时间运行的可执行应用程序。
ms.date: 03/30/2017
f1_keywords:
- ServiceController
helpviewer_keywords:
- Windows Service applications, deploying
- OnStop method
- OnPause method
- services, about services
- Service class, Windows Service applications
- framework services, creating services
- ServiceController components, about Windows services
- Win32OwnProcess service type
- services, lifetime
- OnContinue method
- Windows Service applications, about Windows Service applications
- services, states
- service states
- WaitForStatus method
- Win32ShareProcess service type
- Windows Service applications, lifetime
ms.assetid: 1b1b5e67-3ff3-40c0-8154-322cfd6ef0ae
ms.openlocfilehash: fd69ca11d42a229b861bafd642383e89f0119815
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270466"
---
# <a name="introduction-to-windows-service-applications"></a>Windows 服务应用程序介绍

Microsoft Windows 服务（过去称为 NT 服务）允许用户创建可在其自身的 Windows 会话中长时间运行的可执行应用程序。 这些服务可在计算机启动时自动启动，可以暂停和重启，并且不显示任何用户界面。 这些功能使服务非常适合在服务器上使用，或者需要长时间运行的功能（不会影响在同一台计算机上工作的其他用户）的情况。 还可以在与登录用户或默认计算机帐户不同的特定用户帐户的安全性上下文中运行服务。 有关服务和 Windows 会话的详细信息，请参阅 Windows SDK 文档。  
  
 可以通过创建作为服务安装的应用程序来轻松创建服务。 例如，假设你想监视性能计数器数据并对阈值作出响应。 可以编写一个侦听性能计数器数据的 Windows 服务应用程序，部署该应用程序并开始收集和分析数据。  
  
 可以将服务创建为 Microsoft Visual Studio 项目，并在其中定义代码，以控制可以将哪些命令发送到服务以及在收到这些命令时应采取的操作。 可以发送到服务的命令包括启动、暂停、恢复和停止服务，还可以执行自定义命令。  
  
 在创建和生成应用程序之后，可以通过运行命令行实用程序 InstallUtil.exe 并将该路径传递给服务的可执行文件来安装它。 然后，可以使用服务控制管理器  来启动、停止、暂停、恢复和配置服务。 还可以在“服务器资源管理器”  的“服务”  节点中或使用 <xref:System.ServiceProcess.ServiceController> 类完成许多相同的任务。  
  
## <a name="service-applications-vs-other-visual-studio-applications"></a>服务应用程序与其他 Visual Studio 应用程序  

 服务应用程序与许多其他项目类型的运行方式存在以下几个方面的不同：  
  
- 在项目能够以有意义的方式运行之前，服务应用程序项目创建的已编译可执行文件必须安装在服务器上。 无法通过按 F5 或 F11 来调试或运行服务应用程序；无法立即运行服务或单步执行其代码。 相反，必须安装并启动服务，然后将调试程序附加到服务进程。 有关详细信息，请参阅[如何：调试 Windows 服务应用程序](how-to-debug-windows-service-applications.md)。  
  
- 与某些项目类型不同，你必须为服务应用程序创建安装组件。 安装组件在服务器上安装并注册该服务，并使用 Windows 服务控制管理器  为服务创建条目。 有关详细信息，请参阅[如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)。  
  
- 服务应用程序的 `Main` 方法必须为项目包含的服务发出 Run 命令。 `Run` 方法将服务加载到相应服务器上的服务控制管理器  中。 如果使用 Windows 服务  项目模板，则会自动为你编写此方法。 请注意，加载服务与启动服务不同。 有关详细信息，请参阅下面的“服务生存期”。  
  
- Windows 服务应用程序在不同于登录用户的交互式工作站的窗口站中运行。 窗口站是一个安全对象，它包含一个剪贴板、一个全局原子集和一组桌面对象。 由于 Windows 服务站不是交互式工作站，因此从 Windows 服务应用程序中引发的对话框将不会显示，并且可能导致程序停止响应。 同样，错误消息应记录在 Windows 事件日志中，而不是在用户界面中引发。  
  
     .NET Framework 支持的 Windows 服务类不支持与交互式工作站（即登录用户）的交互。 .NET Framework 也不包括表示工作站和桌面的类。 如果 Windows 服务必须与其他工作站进行交互，则需要访问非管理的 Windows API。 有关详细信息，请参阅 Windows SDK 文档。  
  
     Windows 服务与用户或其他工作站的交互必须经过精心设计，以便包括各种场景（如没有登录用户或用户具有一组意外的桌面对象）。 在某些情况下，编写在用户控制下运行的 Windows 应用程序可能更合适。  
  
- Windows 服务应用程序在其自己的安全上下文中运行，并在用户登录安装这些应用程序的 Windows 计算机之前启动。 应仔细规划要在哪个用户帐户中运行服务；在系统帐户下运行的服务具有比用户帐户更多的权限和特权。  
  
## <a name="service-lifetime"></a>服务生存期  

 一项服务在其生存期内会经历几个内部状态。 首先，服务会安装到它将在其上运行的系统上。 此过程执行服务项目的安装程序，并将该服务加载到该计算机的服务控制管理器  中。 服务控制管理器是 Windows 提供的用于管理服务的中央实用程序。  
  
 必须在服务加载完成后启动它。 启动该服务以允许它开始运行。 可以从服务“服务控制管理器”  、“服务器资源管理器”  ，或从通过调用 <xref:System.ServiceProcess.ServiceController.Start%2A> 方法的代码来启动服务。 <xref:System.ServiceProcess.ServiceController.Start%2A> 方法将处理进程传递给应用程序的 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法，并处理在那里定义的任何代码。  
  
 正在运行的服务可以在此状态下无限期地存在，直到它停止或暂停，或者直到计算机关闭。 服务可以三种基本状态之一存在：<xref:System.ServiceProcess.ServiceControllerStatus.Running>、<xref:System.ServiceProcess.ServiceControllerStatus.Paused> 或 <xref:System.ServiceProcess.ServiceControllerStatus.Stopped>。 该服务还可以报告挂起命令的状态：<xref:System.ServiceProcess.ServiceControllerStatus.ContinuePending>、<xref:System.ServiceProcess.ServiceControllerStatus.PausePending>、<xref:System.ServiceProcess.ServiceControllerStatus.StartPending> 或 <xref:System.ServiceProcess.ServiceControllerStatus.StopPending>。 这些状态指示命令已发出（例如，暂停正在运行的服务的命令），但尚未执行。 可以查询 <xref:System.ServiceProcess.ServiceController.Status%2A> 以确定服务所处的状态，或者在出现其中任一状态时使用 <xref:System.ServiceProcess.ServiceController.WaitForStatus%2A> 执行操作。  
  
 可以从“服务控制管理器”  、“服务器资源管理器”  ，或通过调用代码中的方法来暂停、停止或恢复服务。 其中的每个操作都可以调用服务中的相关过程（<xref:System.ServiceProcess.ServiceBase.OnStop%2A>、<xref:System.ServiceProcess.ServiceBase.OnPause%2A> 或 <xref:System.ServiceProcess.ServiceBase.OnContinue%2A>），可以在其中定义在服务更改状态时执行的其他处理进程。  
  
## <a name="types-of-services"></a>服务类型  

 可以使用 .NET Framework 在 Visual Studio 中创建两种服务类型。 作为进程中唯一服务的服务将分配类型 <xref:System.ServiceProcess.ServiceType.Win32OwnProcess>。 与其他服务共享进程的服务将分配类型 <xref:System.ServiceProcess.ServiceType.Win32ShareProcess>。 可以通过查询 <xref:System.ServiceProcess.ServiceController.ServiceType%2A> 属性来检索服务类型。  
  
 如果查询未在 Visual Studio 中创建的现有服务，则可能偶尔会看到其他服务类型。 有关这些服务类型的更多信息，请参阅 <xref:System.ServiceProcess.ServiceType>。  
  
## <a name="services-and-the-servicecontroller-component"></a>服务和 ServiceController 组件  

 <xref:System.ServiceProcess.ServiceController> 组件用于连接到已安装的服务并操纵其状态；可以使用 <xref:System.ServiceProcess.ServiceController> 组件启动和停止服务，暂停并继续其运行，并将自定义命令发送到服务。 但是，在创建服务应用程序时，无需使用 <xref:System.ServiceProcess.ServiceController> 组件。 事实上，在大多数情况下，<xref:System.ServiceProcess.ServiceController> 组件应存在于定义服务的 Windows 服务应用程序的单独应用程序中。  
  
 有关详细信息，请参阅 <xref:System.ServiceProcess.ServiceController>。  
  
## <a name="requirements"></a>要求  
  
- 必须在 Windows 服务  应用程序项目或其他 .NET Framework（启用了在生成并从 <xref:System.ServiceProcess.ServiceBase> 类继承时创建 .exe 文件的项目）中创建服务。  
  
- 包含 Windows 服务的项目必须具有项目及其服务的安装组件。 这可以通过“属性”  窗口轻松完成。 有关详细信息，请参阅[如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序](index.md)
- [服务应用程序编程体系结构](service-application-programming-architecture.md)
- [如何：创建 Windows 服务](how-to-create-windows-services.md)
- [如何：安装和卸载服务](how-to-install-and-uninstall-services.md)
- [如何：启动服务](how-to-start-services.md)
- [如何：调试 Windows 服务应用程序](how-to-debug-windows-service-applications.md)
- [演练：在组件设计器中创建 Windows 服务应用程序](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)
- [如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)
