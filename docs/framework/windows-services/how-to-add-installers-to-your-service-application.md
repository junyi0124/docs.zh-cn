---
title: 如何：将安装程序添加到服务应用程序
description: 了解如何将安装程序添加到服务应用程序。 Visual Studio 提供可安装与服务应用关联资源的安装组件。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Service applications, deploying
- installation components, adding to services
- installers, adding to services
- Windows Service applications, adding installers
- services, adding installers
- ServiceInstaller class, adding installers to services
- ServiceProcessInstaller class, adding installers to services
ms.assetid: 8b698e9a-b88e-4f44-ae45-e0c5ea0ae5a8
ms.openlocfilehash: 451f0db21e80dfc3dc40052179ac4ec60c2aabdc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270662"
---
# <a name="how-to-add-installers-to-your-service-application"></a>如何：将安装程序添加到服务应用程序

Visual Studio 提供可安装与服务应用程序关联资源的安装组件。 安装组件在安装它的系统上注册单个服务，并让服务控制管理器知道存在该服务。 当使用服务应用程序时，你可以在“属性”窗口中选择一个链接，以将相应的安装程序自动添加到项目中。  
  
> [!NOTE]
> 将服务的属性值从服务类复制到安装程序类。 如果更新服务类的属性值，这些值不会在安装程序中自动更新。  
  
 将安装程序添加到项目时，会在项目中创建一个新类（默认情况下名为 `ProjectInstaller`），并在其中创建适当安装组件的实例。 该类用作项目所需的所有安装组件的中心点。 例如，如果向应用程序添加第二项服务并单击“添加安装程序”链接，则不会创建第二个安装程序类；相反，会将第二项服务必需的附加安装组件添加到现有类中。  
  
 无需在安装程序中执行任何特殊编码就可以正确安装服务。 但是，如果需要为安装进程添加特殊功能，则可能偶尔需要修改安装程序的内容。  
  
> [!NOTE]
> 显示的对话框和菜单命令可能会与“帮助”中的描述不同，具体取决于你现用的设置或版本。 若要更改设置，请在 **“工具”** 菜单上选择 **“导入和导出设置”** 。 有关详细信息，请参阅[个性化设置 Visual Studio IDE](/visualstudio/ide/personalizing-the-visual-studio-ide)。  
  
### <a name="to-add-installers-to-your-service-application"></a>将安装程序添加到服务应用程序  
  
1. 在“解决方案资源管理器”  中，访问要为其添加安装组件的服务的“设计”  视图。  
  
2. 单击设计器的背景以选择服务本身，而不是它的任何内容。  
  
3. 设计器具有焦点时，右键单击，然后单击“添加安装程序”  。  
  
     将新类 `ProjectInstaller` 和两个安装组件 <xref:System.ServiceProcess.ServiceProcessInstaller> 和 <xref:System.ServiceProcess.ServiceInstaller> 添加到项目中，并将该服务的属性值复制到组件。  
  
4. 单击 <xref:System.ServiceProcess.ServiceInstaller> 组件并验证 <xref:System.ServiceProcess.ServiceInstaller.ServiceName%2A> 属性的值是否设置为与服务本身的 <xref:System.ServiceProcess.ServiceInstaller.ServiceName%2A> 属性相同的值。  
  
5. 要确定服务的启动方式，请单击 <xref:System.ServiceProcess.ServiceInstaller> 组件并将 <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> 属性设置为适当的值。  
  
    |“值”|结果|  
    |-----------|------------|  
    |<xref:System.ServiceProcess.ServiceStartMode.Manual>|该服务必须在安装后手动启动。 有关详细信息，请参阅[如何：启动服务](how-to-start-services.md)。|  
    |<xref:System.ServiceProcess.ServiceStartMode.Automatic>|只要重启计算机，服务就将自行启动。|  
    |<xref:System.ServiceProcess.ServiceStartMode.Disabled>|服务无法启动。|  
  
6. 要确定服务将在其中运行的安全性上下文，请单击 <xref:System.ServiceProcess.ServiceProcessInstaller> 组件并设置适当的属性值。 有关详细信息，请参阅[如何：为服务指定安全上下文](how-to-specify-the-security-context-for-services.md)。  
  
7. 替代需要为其执行自定义处理进程的任何方法。  
  
8. 对项目中的每项其他服务执行步骤 1 至 7。  
  
    > [!NOTE]
    > 对于项目中的每项其他服务，必须向项目的 `ProjectInstaller` 类添加一个附加 <xref:System.ServiceProcess.ServiceInstaller> 组件。 在步骤 3 中添加的 <xref:System.ServiceProcess.ServiceProcessInstaller> 组件可与项目中的所有单个服务安装程序一起使用。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：安装和卸载服务](how-to-install-and-uninstall-services.md)
- [如何：启动服务](how-to-start-services.md)
- [如何：为服务指定安全上下文](how-to-specify-the-security-context-for-services.md)
