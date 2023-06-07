---
title: 如何：暂停 Windows 服务 (Visual Basic)
description: 了解如何通过 Visual Basic 使用 ServiceController 组件暂停本地计算机上的 Windows 服务（如 IIS 管理服务）。
ms.date: 03/30/2017
dev_langs:
- vb
f1_keywords:
- ServiceController.Pause
helpviewer_keywords:
- Windows Service applications, pausing
- pausing Windows Service applications
ms.assetid: eddb9409-942b-46b6-a2ce-fbd4c65f2790
ms.openlocfilehash: a8c3074fdbf8d7b948b00def4e6a664310fc6ec2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270584"
---
# <a name="how-to-pause-a-windows-service-visual-basic"></a>如何：暂停 Windows 服务 (Visual Basic)

此示例使用 <xref:System.ServiceProcess.ServiceController> 组件暂停本地计算机上的 IIS 管理服务。  
  
## <a name="example"></a>示例  

 [!code-vb[VbRadconService#11](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#11)]  
[!code-vb[VbRadconService#12](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#12)]  
  
 此代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，它位于“Windows 操作系统 > Windows 服务”  中。 有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="compiling-the-code"></a>编译代码  

 此示例需要：  
  
- 对 System.serviceprocess.dll 的项目引用。  
  
- 对 <xref:System.ServiceProcess> 命名空间成员的访问权限。 如果未在代码中完全限定成员名称，则添加 `Imports` 语句。 有关详细信息，请参阅 [Imports 语句（.NET 命名空间和类型）](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)。  
  
## <a name="robust-programming"></a>可靠编程  

 默认情况下，<xref:System.ServiceProcess.ServiceController> 类的 <xref:System.ServiceProcess.ServiceController.MachineName%2A> 属性是本地计算机。 要在其他计算机上引用 Windows 服务，请将 <xref:System.ServiceProcess.ServiceController.MachineName%2A> 属性更改为该计算机的名称。  
  
 以下情况可能会导致异常：  
  
- 服务无法暂停。 (<xref:System.InvalidOperationException>)  
  
- 访问 API 时出错。 (<xref:System.ComponentModel.Win32Exception>)  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 通过使用 <xref:System.ServiceProcess.ServiceControllerPermissionAccess> 在 <xref:System.ServiceProcess.ServiceControllerPermission> 中设置权限，可以限制计算机上的服务控制。  
  
 通过使用 <xref:System.Security.Permissions.PermissionState> 在 <xref:System.Security.Permissions.SecurityPermission> 中设置权限，可以限制访问服务信息。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceProcess.ServiceController>
- <xref:System.ServiceProcess.ServiceControllerStatus>
- <xref:System.ServiceProcess.ServiceController.WaitForStatus%2A>
- [如何：继续 Windows 服务 (Visual Basic)](how-to-continue-a-windows-service-visual-basic.md)
