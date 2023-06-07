---
title: 保证客户端应用程序的安全
ms.date: 03/30/2017
ms.assetid: 6239592e-fa7d-4dea-9f00-d296d0048b01
ms.openlocfilehash: 96b43d28d3e22df66cb7f7010916b5c7f7a86b77
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189001"
---
# <a name="secure-client-applications"></a>保证客户端应用程序的安全

应用程序通常由多个部件组成，所有这些部件均不能包含漏洞，否则可能导致数据丢失或以其他方式危害系统。 在攻击者可访问数据或系统资源之前将其阻止，以此可创建安全的用户界面来防止出现许多问题。  
  
## <a name="validate-user-input"></a>验证用户输入  

 在构建访问数据的应用程序时，除非能够证明其不为恶意输入，否则您应该假定所有用户输入均为恶意输入。 如果未能实现此目的，则可能会使您的应用程序易于受到攻击。 .NET Framework 包含的类可帮助您约束输入控件的值的域，如限制可输入字符的数目。 使用事件挂钩，您可以编写用于检查值的有效性的程序。 可验证和强类型化用户输入数据，从而限制应用程序对脚本和 SQL 注入攻击的公开程度。  
  
> [!IMPORTANT]
> 您还必须验证数据源和客户端应用程序中的用户输入。 因为攻击者可能选择避开应用程序，而直接攻击数据源。  
  
 [安全性和用户输入](../../../standard/security/security-and-user-input.md)  
 描述如何处理与用户输入相关的细微和潜在危险缺陷。  
  
 [验证 ASP.NET 网页中的用户输入](/previous-versions/aspnet/7kh55542(v=vs.100))  
 概述如何使用 ASP.NET 验证控件验证用户输入。  
  
 [Windows 窗体中的用户输入](/dotnet/desktop/winforms/user-input-in-windows-forms)  
 提供用于验证 Windows 窗体应用程序中鼠标和键盘输入的链接和信息。  
  
 [.NET Framework 正则表达式](../../../standard/base-types/regular-expressions.md)  
 描述如何使用 <xref:System.Text.RegularExpressions.Regex> 类检查用户输入的有效性。  
  
## <a name="windows-applications"></a>Windows 应用程序  

 过去，Windows 应用程序通常使用完全权限运行。 通过使用代码启用安全性 (CAS)，.NET Framework 可提供限制在 Windows 应用程序中执行代码的基础结构。 但是，仅使用 CAS 是不足以保护应用程序的。  
  
 [Windows 窗体安全](/dotnet/desktop/winforms/windows-forms-security)  
 讨论如何保证 Windows 窗体应用程序的安全并提供相关主题的链接。  
  
 [Windows 窗体和非托管应用程序](/dotnet/desktop/winforms/advanced/windows-forms-and-unmanaged-applications)  
 描述如何与 Windows 窗体应用程序中的非托管应用程序进行交互。  
  
 [Windows 窗体的 ClickOnce 部署](/dotnet/desktop/winforms/clickonce-deployment-for-windows-forms)  
 描述如何在 Windows 窗体应用程序中使用 `ClickOnce` 部署，并讨论安全含义。  
  
## <a name="aspnet-and-xml-web-services"></a>ASP.NET 和 XML Web services  

 ASP.NET 应用程序通常需要限制对 Web 站点某些部分的访问，并提供其他数据保护机制和站点安全机制。 这些链接可提供用于保护 ASP.NET 应用程序的有用信息。  
  
 XML Web services 能够提供可由 ASP.NET 应用程序、Windows 窗体应用程序或其他 Web 服务使用的数据。 你需要管理 Web 服务自身的安全性以及客户端应用程序的安全性。  
  
 有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[保护 ASP.NET 网站](/previous-versions/aspnet/91f66yxt(v=vs.100))|讨论如何保证 ASP.NET 应用程序的安全。|  
|[保证使用 ASP.NET 创建的 XML Web services 的安全](/previous-versions/dotnet/netframework-4.0/w67h0dw7(v=vs.100))|讨论如何实现 ASP.NET Web 服务的安全性。|  
|[脚本攻击概述](/previous-versions/aspnet/w1sw53ds(v=vs.100))|讨论如何抵御脚本攻击，该攻击会尝试将恶意字符插入网页。|  
|[Basic Security Practices for Web Applications（Web 应用程序的基本安全实践）](/previous-versions/aspnet/zdh19h94(v=vs.100))|常规安全信息和指向更多讨论的链接，|  
  
## <a name="remoting"></a>远程处理  

 利用 .NET 远程处理，可以方便地生成广泛分布的应用程序，而不论应用程序组件是全部集中在一台计算机上，还是分布在世界各地。 生成的客户端应用程序可以使用同一台计算机（或可通过其网络连接到的任何其他计算机）上其他进程中的对象。 您还可以在同一进程中使用 .NET 远程处理与其他应用程序域进行通信。  
  
|资源|说明|  
|--------------|-----------------|  
|[远程应用程序的配置](/previous-versions/dotnet/netframework-4.0/b8tysty8(v=vs.100))|讨论如何配置远程处理应用程序以避免出现常见问题。|  
|[远程处理中的安全性](/previous-versions/dotnet/netframework-4.0/9hwst9th(v=vs.100))|描述身份验证和加密以及与远程处理相关的其他安全主题。|  
|[安全性和远程处理注意事项](../../misc/security-and-remoting-considerations.md)|描述与受保护对象和应用程序域交叉相关的安全问题。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](securing-ado-net-applications.md)
- [数据访问策略的建议](/previous-versions/visualstudio/visual-studio-2008/8fxztkff(v=vs.90))
- [保护应用程序](/visualstudio/ide/securing-applications)
- [保护连接信息](protecting-connection-information.md)
- [ADO.NET 概述](ado-net-overview.md)
