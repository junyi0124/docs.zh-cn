---
title: 在 Windows 进程激活服务中承载
description: 了解如何管理包含承载 WCF 服务的应用程序的工作进程的激活和生存期。
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF], WAS
ms.assetid: d2b9d226-15b7-41fc-8c9a-cb651ac20ecd
ms.openlocfilehash: 3c0549d93d7898b1d49b31c1a5fde4af71c4d5a9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243002"
---
# <a name="hosting-in-windows-process-activation-service"></a>在 Windows 进程激活服务中承载

Windows 进程激活服务 (WAS) 管理辅助进程的激活和生命周期，这些进程包含托管 Windows Communication Foundation (WCF) 服务的应用程序。 WAS 进程模型通过删除对 HTTP 的依赖，将 HTTP 服务器的 IIS 6.0 进程模型通用化。 这使 WCF 服务可以在支持基于消息的激活的宿主环境中同时使用 HTTP 和非 HTTP 协议（例如 Net.tcp），并提供在给定计算机上托管大量应用程序的功能。  
  
 有关生成在 WAS 宿主环境中运行的 WCF 服务的详细信息，请参阅 [如何：在 was 中承载 Wcf 服务](how-to-host-a-wcf-service-in-was.md)。  
  
 WAS 进程模型提供了一些功能，可以以一种更为可靠、更易管理并有效地使用资源的方式承载应用程序：  
  
- 基于消息的应用程序激活和辅助进程应用程序会动态地启动和停止，以响应使用 HTTP 和非 HTTP 网络协议送达的传入工作项。  
  
- 可靠的应用程序和辅助进程回收可以使应用程序保持良好的运行状况。  
  
- 集中的应用程序配置和管理。  
  
- 允许应用程序利用 IIS 进程模型，而无需完全 IIS 安装的部署需求量。  
[Windows Server AppFabric](/previous-versions/appfabric/ff384253(v=azure.10)) 使用 IIS 7.0 和 Windows 进程激活服务 () 为 NET4 WCF 和 WF 服务提供丰富的应用程序宿主环境。 这些优点包括进程生命周期管理、进程回收、共享承载、快速失败保护、进程孤立、按需激活和运行状况监视。 有关详细信息，请参阅 [Appfabric 托管功能](/previous-versions/appfabric/ee677189(v=azure.10)) 和 [appfabric 托管概念](/previous-versions/appfabric/ee677371(v=azure.10))。  
  
## <a name="elements-of-the-was-addressing-model"></a>WAS 寻址模型的元素  

 应用程序具有统一资源标识符 (URI) 地址，这些地址是一些代码单元，其生存期和执行环境由服务器管理。 一个 WAS 服务器实例可以承载多个不同的应用程序。 服务器将应用程序组织到称为 *站点* 的组。 在网站中，应用程序是以分层的方式排列的，这种方式反映了充当其外部地址的 URI 的结构。  
  
 应用程序地址分为两个部分：基本 URI 前缀和应用程序特定的相对地址（路径）。这两个部分结合在一起时可提供应用程序的外部地址。 基本 URI 前缀从网站绑定构造的，并且适用于网站中的所有应用程序。 然后，将使用应用程序特定的路径 (片段来构造应用程序地址，如 "/applicationOne" ) 并将它们追加到基本 URI 前缀 (例如，"net.tcp：//localhost" ) 到达完整的应用程序 URI。  
  
 下表演示了使用 HTTP 和非 HTTP 网站绑定的 WAS 网站的几个可能的寻址方案。  
  
|场景|网站绑定|应用程序路径|基应用程序 URI|  
|--------------|-------------------|----------------------|---------------------------|  
|仅 HTTP|http： *：80：\*|/appTwo|`http://localhost/appTwo/`|  
|HTTP 和非 HTTP|http： *：80：\*<br /><br /> net.tcp：808：\*|/appTwo|`http://localhost/appTwo/`<br />`net.tcp://localhost/appTwo/`|  
|仅非 HTTP|net.pipe: *|/appThree|`net.pipe://appThree/`|  
  
 也可以对应用程序内的服务和资源进行寻址。 在应用程序内，相对于基应用程序路径对应用程序资源进行寻址。 例如，假定计算机上名为 contoso.com 的网站同时具有 HTTP 和 Net.TCP 协议的网站绑定。 还假定该网站包含一个位于 /Billing 处的应用程序，该应用程序在 GetOrders.svc 中公开服务。 然后，如果 GetOrders.svc 服务使用 SecureEndpoint 的相对地址公开了一个终结点，则将会在下面的两个 URI 中公开该服务终结点：  
  
- `http://contoso.com/Billing/GetOrders.svc/SecureEndpoint`
- `net.tcp://contoso.com/Billing/GetOrders.svc/SecureEndpoint`
  
## <a name="the-was-runtime"></a>WAS 运行库  

 为了便于寻址和管理，可将应用程序组织到网站中。 运行时，还会将应用程序分组到应用程序池中。 一个应用程序池可以存放多个来自许多不同网站的应用程序。 一个应用程序池中的所有应用程序共享一组公共的运行时特征。 例如，它们都在公共语言运行库 (CLR) 的同一版本下运行，并都共享一个公共的进程标识。 每个应用程序池都与辅助进程 (w3wp.exe) 的某个实例相对应。 通过 CLR AppDomain，在共享应用程序池内运行的每个托管应用程序都独立于其他的应用程序。  
  
## <a name="see-also"></a>另请参阅

- [WAS 激活体系结构](was-activation-architecture.md)
- [配置 WAS 以用于 WCF](configuring-the-wpa--service-for-use-with-wcf.md)
- [如何：安装和配置 WCF 激活组件](how-to-install-and-configure-wcf-activation-components.md)
- [如何：在 WAS 中承载 WCF 服务](how-to-host-a-wcf-service-in-was.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
