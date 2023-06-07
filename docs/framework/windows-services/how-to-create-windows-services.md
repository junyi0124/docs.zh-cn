---
title: 如何：创建 Windows 服务
description: 使用 Windows 服务项目模板创建服务。 设置 ServiceName 属性，创建安装程序并重写 OnStart 和 OnStop 方法。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Service applications, creating
- templates, Windows Service
ms.assetid: 0f5e2cbb-d95d-477c-b2b5-4b990e6b86ff
ms.openlocfilehash: 9b171fa54cf65a482625c276c26185b12075c753
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270701"
---
# <a name="how-to-create-windows-services"></a>如何：创建 Windows 服务

创建服务时，可使用名为“Windows 服务”  的 Visual Studio 项目模板。 通过引用适当的类和命名空间、为服务设置来自基类的继承和替代你可能想要替代的几个方法，此模板自动为你完成了许多工作。  
  
> [!WARNING]
> Visual Studio 的速成版中未提供 Windows 服务项目模板。  
  
 要创建功能性服务，你至少必须：  
  
- 设置 <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> 属性。  
  
- 为你的服务应用程序创建必要的安装程序。  
  
- 替代并指定 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 方法的代码，以自定义你的服务的行为方式。  
  
### <a name="to-create-a-windows-service-application"></a>要创建 Windows 服务应用程序  
  
1. 创建“Windows 服务”  项目。  
  
    > [!NOTE]
    > 有关不使用模板编写服务的说明，请参阅[如何：以编程方式编写服务](how-to-write-services-programmatically.md)。  
  
2. 在“属性”  窗口中，为服务设置 <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> 属性。  
  
     ![设置 ServiceName 属性。](./media/windowsservice-servicename.PNG "WindowsService_ServiceName")  
  
    > [!NOTE]
    > <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> 属性的值必须始终与记录在安装程序类中的名称相匹配。 如果更改此属性，你还必须更新安装程序类的 <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> 属性。  
  
3. 设置下列任何一个属性，确定你的服务的运行方式。  
  
    |Property|设置|  
    |--------------|-------------|  
    |<xref:System.ServiceProcess.ServiceBase.CanStop%2A>|`True` 表示服务将接受请求停止运行；`false` 将阻止服务被停止。|  
    |<xref:System.ServiceProcess.ServiceBase.CanShutdown%2A>|`True` 表示当服务所在的计算机关机时服务需要接受通知，启用它来调用 <xref:System.ServiceProcess.ServiceBase.OnShutdown%2A> 过程。|  
    |<xref:System.ServiceProcess.ServiceBase.CanPauseAndContinue%2A>|`True` 表示服务将接受请求暂停或恢复运行；`false` 将阻止服务被暂停或恢复。|  
    |<xref:System.ServiceProcess.ServiceBase.CanHandlePowerEvent%2A>|`True` 表示服务可处理计算机电源状态更改的通知；`false` 将阻止向服务通知这些更改。|  
    |<xref:System.ServiceProcess.ServiceBase.AutoLog%2A>|`True` 将在你的服务执行操作时向应用程序事件日志写入信息条目；`false` 将禁用该功能。 有关详细信息，请参阅[如何：记录关于服务的信息](how-to-log-information-about-services.md)。 **注意：** 默认情况下，<xref:System.ServiceProcess.ServiceBase.AutoLog%2A> 设置为 `true`。|  
  
    > [!NOTE]
    > 当 <xref:System.ServiceProcess.ServiceBase.CanStop%2A> 或 <xref:System.ServiceProcess.ServiceBase.CanPauseAndContinue%2A> 设置为 `false` 时，“服务控制管理器”  将禁用相应的菜单选项来停止、暂停或继续该服务。  
  
4. 访问代码编辑器，并填写你想要对 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 过程的处理。  
  
5. 替代你想要定义功能的任何其他方法。  
  
6. 添加服务应用程序所必需的安装程序。 有关详细信息，请参阅[如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)。  
  
7. 通过从“生成”  菜单选择“生成解决方案”  来生成项目。  
  
    > [!NOTE]
    > 不要通过按 F5 来运行你的项目 — 你无法通过这种方式运行服务项目。  
  
8. 安装服务。 有关详细信息，请参阅[如何：安装和卸载服务](how-to-install-and-uninstall-services.md)。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：以编程方式编写服务](how-to-write-services-programmatically.md)
- [如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)
- [如何：记录关于服务的信息](how-to-log-information-about-services.md)
- [如何：启动服务](how-to-start-services.md)
- [如何：为服务指定安全上下文](how-to-specify-the-security-context-for-services.md)
- [如何：安装和卸载服务](how-to-install-and-uninstall-services.md)
- [演练：在组件设计器中创建 Windows 服务应用程序](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)
