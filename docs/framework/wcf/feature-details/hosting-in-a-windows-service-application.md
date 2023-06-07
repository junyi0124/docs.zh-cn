---
title: 在 Windows 服务应用程序中承载
ms.date: 03/30/2017
ms.assetid: f4199998-27f3-4dd9-aee4-0a4addfa9f24
ms.openlocfilehash: 9f5c78adad34b5fed53a50e85f0361eef469de99
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243067"
---
# <a name="hosting-in-a-windows-service-application"></a>在 Windows 服务应用程序中承载

Windows 服务（以前称为 Windows NT 服务）提供了一种尤其适合于下面这样的应用程序的进程模型：必须在长时间运行的可执行程序中生存，并且不显示任何形式的用户界面。 Windows 服务应用程序的进程生存期由服务控制管理器 (SCM) 管理，您可以通过该管理器启动、停止和暂停 Windows 服务应用程序。 你可以将 Windows 服务进程配置为在计算机启动时自动启动，使其成为适用于 "alwayson" 应用程序的合适宿主环境。 有关 Windows 服务应用程序的详细信息，请参阅 [Windows 服务应用程序](https://go.microsoft.com/fwlink/?LinkId=89450)。  
  
 承载长时间运行 Windows Communication Foundation 的应用程序 (WCF) 服务与 Windows 服务共享许多特性。 特别是，WCF 服务是长时间运行的服务器可执行文件，这些可执行文件不会直接与用户交互，因此不实现任何形式的用户界面。 同样，在 Windows 服务应用程序中托管 WCF 服务是一种用于生成可靠、长时间运行的 WCF 应用程序的选项。  
  
 通常情况下，WCF 开发人员必须决定是在 Windows 服务应用程序内还是宿主 Internet Information Services (IIS) 或 Windows 进程激活服务 (是) 宿主环境。 在下列条件下，应考虑使用 Windows 服务应用程序：  
  
- 应用程序要求显式激活。 例如，在服务器启动时应用程序必须自动启动（而不是动态启动以响应第一个传入消息）的条件下，应使用 Windows 服务。  
  
- 承载应用程序的进程必须在启动后保持运行状态。 Windows 服务进程一旦启动将一直保持运行状态，直到服务器管理员使用服务控制管理器显式关闭该进程。 在 IIS 或 WAS 中承载的应用程序可能会动态地启动和停止，以便最佳地使用系统资源。 需要显式控制其托管进程的生存期的应用程序应使用 Windows 服务而非 IIS 或 WAS。  
  
- WCF 服务必须在 Windows Server 2003 上运行，并使用 HTTP 以外的其他传输。 在 Windows Server 2003 上，IIS 6.0 宿主环境仅限于 HTTP 通信。 Windows 服务应用程序不受此限制的限制，可以使用任何传输 WCF 支持，包括 net.tcp、net.pipe 和 net.tcp。  
  
### <a name="to-host-wcf-inside-of-a-windows-service-application"></a>在 Windows 服务应用程序内承载 WCF  
  
1. 创建 Windows 服务应用程序。 可以使用 <xref:System.ServiceProcess> 命名空间中的类以托管代码的形式编写 Windows 服务应用程序。 此应用程序必须包含一个继承自 <xref:System.ServiceProcess.ServiceBase> 的类。  
  
2. 将 WCF 服务的生存期链接到 Windows 服务应用程序的生存期。 通常，你希望 Windows 服务应用程序中托管的 WCF 服务在宿主服务启动时变为活动状态，停止在托管服务上侦听消息，并在 WCF 服务遇到错误时关闭宿主进程。 这可以通过以下操作实现：  
  
    - 重写 <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> 以打开一个或多个 <xref:System.ServiceModel.ServiceHost> 实例。 单个 Windows 服务应用程序可以托管多个以组的形式启动和停止的 WCF 服务。  
  
    - 重写 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 以调用在 <xref:System.ServiceModel.Channels.CommunicationObject.Closed> <xref:System.ServiceModel.ServiceHost> 中启动的任何正在运行的 WCF 服务 <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> 。  
  
    - 订阅 <xref:System.ServiceModel.Channels.CommunicationObject.Faulted> 的 <xref:System.ServiceModel.ServiceHost> 事件，并使用 <xref:System.ServiceProcess.ServiceController> 类以在出现错误时关闭 Windows 服务应用程序。  
  
     部署和管理承载 WCF 服务的 windows 服务应用程序的方式与未使用 WCF 的 Windows 服务应用程序相同。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceProcess>
- [演练：在组件设计器中创建 Windows 服务应用程序](https://go.microsoft.com/fwlink/?LinkId=94875)
- [如何：在托管 Windows 服务中承载 WCF 服务](how-to-host-a-wcf-service-in-a-managed-windows-service.md)
- [Windows 服务主机](../samples/windows-service-host.md)
- [服务应用程序编程体系结构](https://go.microsoft.com/fwlink/?LinkId=94876)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
