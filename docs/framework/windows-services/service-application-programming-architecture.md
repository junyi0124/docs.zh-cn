---
title: 服务应用程序编程体系结构
description: 了解服务应用程序编程体系结构。 Windows 服务应用程序基于从 System.ServiceProcess.ServiceBase 继承的类。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ServiceController components, programming architecture
- ServiceBase class, service states
- Windows Service applications, code model
- services, programming architecture
- ServiceController class
- services, states
- ServiceProcessInstaller class, service application code model
- Windows Service applications, states
ms.assetid: 83230026-d068-4174-97ff-e264c896eb2f
ms.openlocfilehash: c2344dd0a7d35781aea52b24694f2cfee70a6d41
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270453"
---
# <a name="service-application-programming-architecture"></a>服务应用程序编程体系结构

Windows 服务应用程序基于从 <xref:System.ServiceProcess.ServiceBase?displayProperty=nameWithType> 类继承的类。 可以替代此类中的方法并为其定义功能，以确定服务的行为方式。  
  
 服务创建中涉及的主要类包括：  
  
- <xref:System.ServiceProcess.ServiceBase?displayProperty=nameWithType> — 可以在创建服务时替代 <xref:System.ServiceProcess.ServiceBase> 类中的方法，并定义代码以确定服务在此继承类中的工作方式。  
  
- <xref:System.ServiceProcess.ServiceProcessInstaller?displayProperty=nameWithType> 和 <xref:System.ServiceProcess.ServiceInstaller?displayProperty=nameWithType> — 使用这些类来安装和卸载服务。  
  
 另外，可以使用名为 <xref:System.ServiceProcess.ServiceController> 的类来操纵服务本身。 该类不参与创建服务，但可用于启动和停止服务、将命令传递给它并返回一系列枚举。  
  
## <a name="defining-your-services-behavior"></a>定义服务的行为  

 在服务类中，替代基类函数，这些函数可确定服务状态在服务控制管理器中更改时会发生什么情况。 <xref:System.ServiceProcess.ServiceBase> 类公开以下方法，可替代这些方法以添加自定义行为。  
  
|方法|替代为|  
|------------|-----------------|  
|<xref:System.ServiceProcess.ServiceBase.OnStart%2A>|指示服务开始运行时应采取的操作。 必须在此过程中为服务编写代码才能执行有用的操作。|  
|<xref:System.ServiceProcess.ServiceBase.OnPause%2A>|指示在服务暂停时应发生什么情况。|  
|<xref:System.ServiceProcess.ServiceBase.OnStop%2A>|指示在服务停止运行时应发生什么情况。|  
|<xref:System.ServiceProcess.ServiceBase.OnContinue%2A>|指示服务在暂停后恢复正常运行时应发生什么情况。|  
|<xref:System.ServiceProcess.ServiceBase.OnShutdown%2A>|指示在系统关闭之前应发生什么情况（如果此时服务正在运行）。|  
|<xref:System.ServiceProcess.ServiceBase.OnCustomCommand%2A>|指示服务在收到自定义命令时应发生什么情况。 有关自定义命令的详细信息，请参阅 MSDN Online。|  
|<xref:System.ServiceProcess.ServiceBase.OnPowerEvent%2A>|指示服务在收到电源管理事件时应如何响应，如电池电量不足或已挂起的操作。|  
  
> [!NOTE]
> 这些方法表示服务在其生存期中不断变化的状态；服务从一个状态转换到下一个状态。 例如，在调用 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 之前，将永远无法获得该服务来响应 <xref:System.ServiceProcess.ServiceBase.OnContinue%2A> 命令。  
  
 还有其他几个令人感兴趣的属性和方法。 这些方法包括：  
  
- <xref:System.ServiceProcess.ServiceBase> 类的 <xref:System.ServiceProcess.ServiceBase.Run%2A> 方法。 这是服务的主入口点。 使用 Windows 服务模板创建服务时，将在应用程序的 `Main` 方法中插入代码以运行该服务。 此代码如下所示：  
  
     [!code-csharp[VbRadconService#6](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#6)]
     [!code-vb[VbRadconService#6](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#6)]  
  
    > [!NOTE]
    > 这些示例使用 <xref:System.ServiceProcess.ServiceBase> 类型的数组，可将应用程序包含的每项服务添加到其中，然后所有服务均可一同运行。 但是，如果仅创建单个服务，则可以选择不使用该数组，然后只声明从 <xref:System.ServiceProcess.ServiceBase> 继承的新对象，然后运行它。 有关示例，请参见 [如何：以编程方式编写服务](how-to-write-services-programmatically.md)。  
  
- <xref:System.ServiceProcess.ServiceBase> 类的一系列属性。 这些属性确定可对服务调用的方法。 例如，当 <xref:System.ServiceProcess.ServiceBase.CanStop%2A> 属性设置为 `true` 时，可以调用服务上的 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 方法。 当 <xref:System.ServiceProcess.ServiceBase.CanPauseAndContinue%2A> 属性设置为 `true` 时，可以调用 <xref:System.ServiceProcess.ServiceBase.OnPause%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnContinue%2A> 方法。 在将其中一个属性设置为 `true` 时，应该替代并定义关联方法的处理进程。  
  
    > [!NOTE]
    > 服务必须至少替代 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 才有用。  
  
 还可以使用名为 <xref:System.ServiceProcess.ServiceController> 的组件与现有服务进行通信并控制其行为。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：创建 Windows 服务](how-to-create-windows-services.md)
