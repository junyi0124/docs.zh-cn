---
title: 如何：继续 Windows 服务 (Visual Basic)
description: 了解如何通过 Visual Basic 使用 ServiceController 组件继续本地计算机上的 Windows 服务（如 IIS 管理服务）。
ms.date: 03/30/2017
dev_langs:
- vb
f1_keywords:
- ServiceController.Continue
helpviewer_keywords:
- Windows Service applications, pausing
- pausing Windows Service applications
ms.assetid: e5d13760-4c83-4b0d-abef-39852677cd7a
ms.openlocfilehash: 89313ebec87d154621b23b57bfa1eeda5ac3dee4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270649"
---
# <a name="how-to-continue-a-windows-service-visual-basic"></a>如何：继续 Windows 服务 (Visual Basic)

本示例使用 <xref:System.ServiceProcess.ServiceController> 组件继续在本地计算机上执行 IIS 管理服务。  
  
## <a name="example"></a>示例  

 [!code-vb[VbRadconService#11](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#11)]  
[!code-vb[VbRadconService#13](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#13)]  
  
 此代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，它位于“Windows 操作系统 > Windows 服务”  中。 有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="compiling-the-code"></a>编译代码  

 此示例需要：  
  
- 对 System.serviceprocess.dll 的项目引用。  
  
- 对 <xref:System.ServiceProcess> 命名空间成员的访问权限。 如果未在代码中完全限定成员名称，则添加 `Imports` 语句。 有关详细信息，请参阅 [Imports 语句（.NET 命名空间和类型）](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)。  
  
## <a name="robust-programming"></a>可靠编程  

 默认情况下，<xref:System.ServiceProcess.ServiceController> 类的 <xref:System.ServiceProcess.ServiceController.MachineName%2A> 属性是本地计算机。 要在其他计算机上引用 Windows 服务，请将 <xref:System.ServiceProcess.ServiceController.MachineName%2A> 属性更改为该计算机的名称。  
  
 在服务控制器状态为 <xref:System.ServiceProcess.ServiceControllerStatus.Paused> 之前，无法调用服务上的 <xref:System.ServiceProcess.ServiceController.Continue%2A> 方法。  
  
 以下情况可能会导致异常：  
  
- 服务无法恢复。 (<xref:System.InvalidOperationException>)  
  
- 访问 API 时出错。 (<xref:System.ComponentModel.Win32Exception>)  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 通过使用 <xref:System.ServiceProcess.ServiceControllerPermissionAccess> 枚举在 <xref:System.ServiceProcess.ServiceControllerPermission> 类中设置权限，可以限制计算机上的服务控制。  
  
 通过使用 <xref:System.Security.Permissions.PermissionState> 枚举在 <xref:System.Security.Permissions.SecurityPermission> 类中设置权限，可以限制访问服务信息。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceProcess.ServiceController>
- <xref:System.ServiceProcess.ServiceControllerStatus>
- [如何：暂停 Windows 服务 (Visual Basic)](how-to-pause-a-windows-service-visual-basic.md)
