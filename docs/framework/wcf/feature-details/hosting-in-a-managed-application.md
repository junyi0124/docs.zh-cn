---
title: 在托管应用程序中承载
ms.date: 03/30/2017
ms.assetid: af70132d-e9e1-4f32-b20f-f0014629758a
ms.openlocfilehash: 0f220b3eb249a9615024adbd798db49ea94376eb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289933"
---
# <a name="hosting-in-a-managed-application"></a>在托管应用程序中承载

Windows Communication Foundation 可以在任何 .NET Framework 应用程序中承载 WCF) 服务 (。 自承载服务是最灵活的宿主选项，因为此服务部署所需要的基础结构最少。 不过，它也是最不可靠的宿主选项，因为托管应用程序未提供 WCF 中其他宿主选项（如 Internet Information Services (IIS) 和 Windows 服务）的高级宿主和管理功能。  
  
 若要创建自承载服务，请创建并打开 <xref:System.ServiceModel.ServiceHost>的实例以启动侦听消息的服务。 有关详细信息，请参阅 [如何：在托管应用程序中托管 WCF 服务](../how-to-host-a-wcf-service-in-a-managed-application.md)。  
  
 有关如何定义协定、实现协定以及在托管应用程序内承载服务的完整示例，请参阅 [入门教程](../getting-started-tutorial.md) 和 [自承载](../samples/self-host.md)服务。  
  
 以下各部分描述了使用此宿主选项的常见情况。  
  
## <a name="console-applications"></a>控制台应用程序  

 自承载支持的常见方案是在控制台应用程序内部运行的 WCF 服务。 在控制台应用程序内承载 WCF 服务通常在服务的开发阶段非常有用。 这使服务变得容易调试，从中跟踪信息以查明应用程序内发生的情况变得更加方便，以及通过将其复制到新的位置进行来回移动变得更加轻松。  
  
## <a name="rich-client-applications"></a>胖客户端应用程序  

 自承载支持的其他常见方案是丰富的客户端应用程序，例如基于 Windows Presentation Foundation (WPF) 或 Windows 窗体 (WinForms) 的应用程序。 此宿主选项还使丰富的客户端应用程序（如 WPF 和 WinForms 应用程序）可以方便地与外界通信。 例如，对等协作客户端使用 WPF 作为其用户界面并托管 WCF 服务，该服务允许其他客户端连接到它并共享信息。  
  
## <a name="see-also"></a>另请参阅

- [承载服务](../hosting-services.md)
- [入门教程](../getting-started-tutorial.md)
