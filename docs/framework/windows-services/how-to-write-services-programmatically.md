---
title: 如何：以编程方式编写服务
description: 了解如何通过自行设置继承和其他基础结构元素以编程方式编写服务。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- services, creating
- Windows Service applications, creating
ms.assetid: 3abbb2ec-78d2-41e6-b9f9-6662d4e2cdc7
ms.openlocfilehash: ab153b89272323a1a7a71181559f4f4eee082640
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270506"
---
# <a name="how-to-write-services-programmatically"></a>如何：以编程方式编写服务

如果选择不使用 Windows 服务项目模板，则可以通过自行设置继承和其他基础结构元素来编写自己的服务。 当以编程方式创建服务时，必须执行以下几个步骤（否则，模板将为你处理）：  
  
- 必须将服务类设置为从 <xref:System.ServiceProcess.ServiceBase> 类继承。  
  
- 必须为服务项目创建 `Main` 方法，该方法定义要运行的服务并在其上调用 <xref:System.ServiceProcess.ServiceBase.Run%2A> 方法。  
  
- 必须替代 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 过程并填入你希望它们运行的任何代码。  
  
### <a name="to-write-a-service-programmatically"></a>以编程方式编写服务  
  
1. 创建一个空项目并通过以下步骤创建对必要命名空间的引用：  
  
    1. 在“解决方案资源管理器”  中，右键单击“引用”  节点，然后单击  “添加引用”。  
  
    2. 在“.NET Framework”  选项卡上，滚动到“System.dll”  ，然后单击“选择”  。  
  
    3. 滚动到“System.ServiceProcess.dll”  ，然后单击“选择”  。  
  
    4. 单击 **“确定”** 。  
  
2. 添加一个类并将其配置为从 <xref:System.ServiceProcess.ServiceBase> 继承：  
  
     [!code-csharp[VbRadconService#7](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#7)]
     [!code-vb[VbRadconService#7](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#7)]  
  
3. 添加下面的代码来配置服务类：  
  
     [!code-csharp[VbRadconService#8](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#8)]
     [!code-vb[VbRadconService#8](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#8)]  
  
4. 为类创建 `Main` 方法，并用它来定义类将包含的服务；`userService1` 是类的名称：  
  
     [!code-csharp[VbRadconService#9](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#9)]
     [!code-vb[VbRadconService#9](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#9)]  
  
5. 替代 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法，并定义服务启动时想要执行的任何处理进程。  
  
     [!code-csharp[VbRadconService#10](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#10)]
     [!code-vb[VbRadconService#10](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#10)]  
  
6. 替代要为其定义自定义处理进程的任何其他方法，并编写代码以确定服务在每种情况下应采取的操作。  
  
7. 添加服务应用程序所必需的安装程序。 有关详细信息，请参阅[如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)。  
  
8. 通过从“生成”  菜单选择“生成解决方案”  来生成项目。  
  
    > [!NOTE]
    > 不要通过按 F5 来运行你的项目 — 你无法通过这种方式运行服务项目。  
  
9. 创建安装项目和自定义操作以安装服务。 有关示例，请参阅[演练：在组件设计器中创建 Windows 服务应用程序](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)。  
  
10. 安装服务。 有关详细信息，请参阅[如何：安装和卸载服务](how-to-install-and-uninstall-services.md)。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：创建 Windows 服务](how-to-create-windows-services.md)
- [如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)
- [如何：记录关于服务的信息](how-to-log-information-about-services.md)
- [演练：在组件设计器中创建 Windows 服务应用程序](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)
