---
title: 如何：在 IIS 中承载 WCF 服务
description: 了解如何创建 Internet Information Services (IIS) 中承载的 WCF 服务。 只可以将 IIS 宿主与 HTTP 传输协议一起使用。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b044b1c9-c1e5-4c9f-84d8-0f02f4537f8b
ms.openlocfilehash: e0eb61e56b20eda6627030700b823042e07d10c9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244439"
---
# <a name="how-to-host-a-wcf-service-in-iis"></a>如何：在 IIS 中承载 WCF 服务

本主题概述了在 Internet Information Services (IIS) 中创建 Windows Communication Foundation (WCF) 服务所需的基本步骤。 本主题假设您熟悉 IIS 且了解如何使用 IIS 管理工具创建和管理 IIS 应用程序。 有关 IIS 的详细信息，请参阅 [Internet Information Services](https://www.iis.net/)。 在 IIS 环境中运行的 WCF 服务充分利用 IIS 功能，如进程回收、空闲关闭、进程运行状况监视和基于消息的激活。 此宿主选项要求正确配置 IIS，但不需要编写任何承载代码作为应用程序的一部分。 只可以将 IIS 宿主与 HTTP 传输协议一起使用。  
  
 有关 WCF 和 ASP.NET 如何交互的详细信息，请参阅 [Wcf 服务和 ASP.NET](wcf-services-and-aspnet.md)。 有关配置安全的详细信息，请参阅 [安全性](security.md)。  
  
 有关此示例的源副本，请参阅 [使用内联代码的 IIS 承载](../samples/iis-hosting-using-inline-code.md)。  
  
### <a name="to-create-a-service-hosted-by-iis"></a>创建由 IIS 承载的服务  
  
1. 确认 IIS 已经安装并在计算机上运行。 有关安装和配置 IIS 的详细信息，请参阅 [安装和配置 iis 7.0](/iis/install/installing-iis-7/installing-necessary-iis-components-on-windows-vista)  
  
2. 为应用程序文件创建一个名为 "IISHostedCalcService" 的新文件夹，确保 ASP.NET 有权访问该文件夹的内容，并使用 IIS 管理工具创建一个物理上位于此应用程序目录中的新 IIS 应用程序。 当为应用程序目录创建别名时，请使用“IISHostedCalc”。  
  
3. 在应用程序目录中创建一个名为“service.svc”的新文件。 通过添加以下元素编辑此文件 @ServiceHost 。  
  
   ```aspx-csharp
   <%@ServiceHost language=c# Debug="true" Service="Microsoft.ServiceModel.Samples.CalculatorService"%>
   ```  
  
4. 在应用程序目录中创建 App_Code 子目录。  
  
5. 在 App_Code 子目录中创建名为 Service.cs 的代码文件。  
  
6. 将下面的 using 语句添加到 Service.cs 文件的最前面。  
  
    ```csharp  
    using System;  
    using System.ServiceModel;  
    ```  
  
7. 将下面的命名空间声明添加到 using 语句后面。  
  
    ```csharp  
    namespace Microsoft.ServiceModel.Samples  
    {  
    }  
    ```  
  
8. 定义命名空间声明中的服务协定，如下面的代码所示。  
  
     [!code-csharp[c_HowTo_HostInIIS#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostiniis/cs/source.cs#11)]
     [!code-vb[c_HowTo_HostInIIS#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostiniis/vb/source.vb#11)]  
  
9. 在服务协定定义后实现服务协定，如下面的代码所示。  
  
     [!code-csharp[c_HowTo_HostInIIS#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostiniis/cs/source.cs#12)]
     [!code-vb[c_HowTo_HostInIIS#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostiniis/vb/source.vb#12)]  
  
10. 在应用程序目录中创建一个名为“Web.config”的文件，并将下面的配置代码添加到该文件中。 在运行时，WCF 基础结构使用这些信息来构造客户端应用程序可与其通信的终结点。  
  
     [!code-xml[c_HowTo_HostInIIS#100](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostiniis/common/web.config#100)]
  
     此示例显式指定配置文件中的终结点。 如果您不希望向服务添加任何终结点，则运行时为您添加默认终结点。 有关默认终结点、绑定和行为的详细信息，请参阅适用[于 WCF 服务的](../samples/simplified-configuration-for-wcf-services.md)[简化配置](../simplified-configuration.md)和简化配置。  
  
11. 为了确保正确承载该服务，请打开 Internet Explorer 的实例，导航到该服务的 URL：`http://localhost/IISHostedCalc/Service.svc`  
  
## <a name="example"></a>示例  

 下面是 IIS 承载的计算器服务的代码的完整列表。  
  
 [!code-csharp[C_HowTo_HostInIIS#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostiniis/cs/source.cs#1)]
 [!code-vb[C_HowTo_HostInIIS#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostiniis/vb/source.vb#1)]
 [!code-xml[c_HowTo_HostInIIS#100](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostiniis/common/web.config#100)]  
  
## <a name="see-also"></a>另请参阅

- [在 Internet 信息服务中承载](hosting-in-internet-information-services.md)
- [承载服务](../hosting-services.md)
- [WCF 服务和 ASP.NET](wcf-services-and-aspnet.md)
- [安全性](security.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
