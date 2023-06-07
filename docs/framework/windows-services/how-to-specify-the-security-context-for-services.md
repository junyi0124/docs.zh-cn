---
title: 如何：为服务指定安全上下文
description: 为服务指定安全上下文。 在默认系统帐户上下文中运行的服务具有登录用户以外的其他系统资源访问权限。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Service applications, security
- security [Visual Studio], contexts
- contexts, Visual Studio security
- security [Visual Studio], service applications
- ServiceProcessInstaller class, security context
- services, security
- ServiceInstaller class, security context
ms.assetid: 02187c7b-dbf2-45f2-96c2-e11010225a22
ms.openlocfilehash: dcf0680f1bcb0f0e927bdb37ea56f7b3025be09b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270532"
---
# <a name="how-to-specify-the-security-context-for-services"></a>如何：为服务指定安全上下文

默认情况下，服务在与登录用户不同的安全性上下文中运行。 服务在名为 `LocalSystem` 的默认系统帐户的上下文中运行，这样使服务拥有与用户不同的针对系统资源的访问权限。 可以更改此行为以指定应在其下运行服务的其他用户帐户。  
  
 可以通过操作服务运行于其中的进程的 <xref:System.ServiceProcess.ServiceProcessInstaller.Account%2A> 属性来设置安全性上下文。 此属性允许将服务设置为以下四种帐户类型之一：  
  
- `User`，该帐户会导致系统在安装服务时提示输入有效的用户名和密码，并在网络上单个用户指定的帐户的上下文中运行；  
  
- `LocalService`，该帐户在用作本地计算机上的非特权用户的帐户的上下文中运行，并向任意远程服务器提供匿名凭据；  
  
- `LocalSystem`，该帐户在提供广泛本地权限的帐户的上下文中运行，并向任意远程服务器提供计算机凭据；  
  
- `NetworkService`，该帐户在用作本地计算机上的非特权用户的帐户的上下文中运行，并向任意远程服务器提供计算机凭据。  
  
 有关详细信息，请参见 <xref:System.ServiceProcess.ServiceAccount> 枚举。  
  
### <a name="to-specify-the-security-context-for-a-service"></a>为服务指定安全性上下文  
  
1. 在创建服务后为其添加必要的安装程序。 有关详细信息，请参阅[如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)。  
  
2. 在设计器中，访问 `ProjectInstaller` 类并单击正在使用的服务的服务进程安装程序。  
  
    > [!NOTE]
    > 对于每个服务应用程序，在 `ProjectInstaller` 类中至少有两个安装组件 — 一个用于安装项目中所有服务的进程，另一个是应用程序包含的每个服务的安装程序。 在这种情况下，你要选择 <xref:System.ServiceProcess.ServiceProcessInstaller>。  
  
3. 在“属性”  窗口中，将 <xref:System.ServiceProcess.ServiceProcessInstaller.Account%2A> 设置为适当的值。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)
- [如何：创建 Windows 服务](how-to-create-windows-services.md)
