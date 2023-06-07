---
title: 如何：记录关于服务的信息
description: 了解如何记录有关服务的信息。 如果希望 Windows 服务项目与应用程序事件日志交互，请设置 AutoLog 属性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- AutoLog property
- services, logging information
- Windows Service applications, logging information about
- event logs, service applications
- application event logs, service applications
- logs, service applications
ms.assetid: c0d8140f-c055-4d8e-a2e0-37358a550116
ms.openlocfilehash: 2e5f1fd8ebbbb218e8d6eba9b2d30d05e7c0e62c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270571"
---
# <a name="how-to-log-information-about-services"></a>如何：记录关于服务的信息

默认情况下，所有 Windows 服务项目都具有与应用程序事件日志进行交互并向其中写入信息和异常的功能。 使用 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 属性可指示是否希望应用程序具有此功能。 默认情况下，用 Windows 服务项目模板创建的所有服务的记录都是打开的。 可以使用静态形式的 <xref:System.Diagnostics.EventLog> 类将服务信息写入日志，而无需创建 <xref:System.Diagnostics.EventLog> 组件的实例或手动注册源。  
  
 当记录打开时，服务的安装程序自动在安装了服务的计算机上的应用程序日志中，将项目中的每项服务注册为一个有效的事件源。 该服务在服务每次启动、停止、暂停、继续、安装和卸载时记录信息。 它还记录发生的所有失败。 当使用默认行为时，不需编写任何代码以便将项写入日志；服务自动为你处理此任务。  
  
 如果要向事件日志而不是应用程序日志中写入，则必须将 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 属性设置为 `false`，在服务代码内创建自定义事件日志，并将服务注册为该日志的有效项源。 然后必须编写代码，以在相关操作发生时将项记入日志。  
  
> [!NOTE]
> 如果使用一个自定义事件日志并配置服务应用程序写入该日志，则在代码中设置服务的 <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> 属性之前，不要尝试访问该事件日志。 事件日志需要此属性的值才可将服务注册为一个有效的事件源。  
  
### <a name="to-enable-default-event-logging-for-your-service"></a>为服务启用默认事件记录  
  
- 将组件的 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 属性设置为 `true`。  
  
    > [!NOTE]
    > 默认情况下，此属性设置为 `true`。 不需要显式设置此属性，除非正在生成更复杂的处理，如计算一个条件然后根据该条件的结果设置 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 属性。  
  
### <a name="to-disable-event-logging-for-your-service"></a>为服务禁用默认事件记录  
  
- 将组件的 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 属性设置为 `false`。  
  
     [!code-csharp[VbRadconService#17](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#17)]
     [!code-vb[VbRadconService#17](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#17)]  
  
### <a name="to-set-up-logging-to-a-custom-log"></a>将记录设置为自定义日志  
  
1. 将 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 属性设置为 `false`。  
  
    > [!NOTE]
    > 若要使用自定义日志，必须将 <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 设置为 false 。  
  
2. 在 Windows 服务应用程序中设置 <xref:System.Diagnostics.EventLog> 组件的一个实例。  
  
3. 通过调用 <xref:System.Diagnostics.EventLog.CreateEventSource%2A> 方法，并指定源字符串和要创建的日志文件的名称，创建一个自定义日志。  
  
4. 将 <xref:System.Diagnostics.EventLog.Source%2A> 组件实例上的 <xref:System.Diagnostics.EventLog> 属性设置为在步骤 3 中创建的源字符串。  
  
5. 通过访问 <xref:System.Diagnostics.EventLog.WriteEntry%2A> 组件实例上的 <xref:System.Diagnostics.EventLog> 方法来编写项。  
  
     下面的代码演示如何将记录设置为自定义日志。  
  
    > [!NOTE]
    > 在此代码示例中，一个 <xref:System.Diagnostics.EventLog> 组件实例命名为 `eventLog1` （在 Visual Basic 中为`EventLog1` ）。 如果在步骤 2 中创建具有另一名称的实例，请相应地更改代码。  
  
     [!code-csharp[VbRadconService#14](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#14)]
     [!code-vb[VbRadconService#14](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#14)]  
    [!code-csharp[VbRadconService#15](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#15)]
    [!code-vb[VbRadconService#15](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#15)]  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
