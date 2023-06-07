---
title: 疑难解答：调试 Windows 服务
description: 开始调试 Windows 服务。 调试 Windows 服务应用程序时，服务和 Windows 服务管理器会交互。
ms.date: 03/30/2017
helpviewer_keywords:
- debugging Windows Service applications
- debugging [Visual Studio], Windows services
- troubleshooting service applications
- services, troubleshooting
- troubleshooting debugging, Windows Services
- Windows Service applications, debugging
- services, debugging
- Windows Service applications, troubleshooting
ms.assetid: cf859d4c-f04c-4cb7-81e3-bc7de8bea190
ms.openlocfilehash: 2c1180ef8e3e44c50f10a263d86dcae830d9cd0e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96270388"
---
# <a name="troubleshooting-debugging-windows-services"></a>疑难解答：调试 Windows 服务

在调试 Windows 服务应用程序时，服务和 Windows 服务管理器  交互。 Service Manager 通过调用 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法来启动服务，然后等待 30 秒以便 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法返回。 如果此时该方法未返回，则管理器将显示服务无法启动的错误。  
  
 按[如何：调试 Windows 服务应用程序](how-to-debug-windows-service-applications.md)中所述调试 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法时，必须注意这 30 秒的时间。 如果在 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法中放置断点并且未在 30 秒内逐步执行该断点，则管理器将不会启动该服务。  
  
## <a name="see-also"></a>请参阅

- [如何：调试 Windows 服务应用程序](how-to-debug-windows-service-applications.md)
- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
