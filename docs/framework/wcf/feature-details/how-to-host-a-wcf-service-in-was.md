---
title: 如何：在 WAS 中承载 WCF 服务
ms.date: 03/30/2017
ms.assetid: 9e3e213e-2dce-4f98-81a3-f62f44caeb54
ms.openlocfilehash: 640cfdd7525fb9877c6f3551a1456fed29c99b8a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244400"
---
# <a name="how-to-host-a-wcf-service-in-was"></a>如何：在 WAS 中承载 WCF 服务

本主题概述了创建 Windows 进程激活服务所需的基本步骤， (也称为) 承载 Windows Communication Foundation (WCF) 服务。 WAS 是新的进程激活服务，是对使用非 HTTP 传输协议的 Internet Information Services (IIS) 功能的泛化。 WCF 使用侦听器适配器接口传递通过 WCF 支持的非 HTTP 协议（如 TCP、命名管道和消息队列）接收的激活请求。  
  
 此主机选项要求正确安装和配置 WAS 激活组件，但不要求编写任何主机代码作为应用程序的一部分。 有关安装和配置 WAS 的详细信息，请参阅 [如何：安装和配置 WCF 激活组件](how-to-install-and-configure-wcf-activation-components.md)。  
  
> [!WARNING]
> 如果将 Web 服务器的请求处理管道设置为经典模式，则将不支持 WAS 激活。 如果要使用 WAS 激活，则必须将 Web 服务器的请求处理管道设置为集成模式。  
  
 当在 WAS 中承载 WCF 服务时，将按常规方式使用标准绑定。 但是，在使用 <xref:System.ServiceModel.NetTcpBinding> 和 <xref:System.ServiceModel.NetNamedPipeBinding> 配置 WAS 承载的服务时，必须满足一个约束条件。 当不同的终结点使用相同的传输时，绑定设置必须在以下的七个属性上相匹配：  
  
- ConnectionBufferSize  
  
- ChannelInitializationTimeout  
  
- MaxPendingConnections  
  
- MaxOutputDelay  
  
- MaxPendingAccepts  
  
- ConnectionPoolSettings.IdleTimeout  
  
- ConnectionPoolSettings.MaxOutboundConnectionsPerEndpoint  
  
 否则，最先初始化的终结点总是确定这些属性的值，并且以后添加的终结点若与这些设置不匹配，则将引发 <xref:System.ServiceModel.ServiceActivationException>。  
  
 有关此示例的源副本，请参阅 [TCP 激活](../samples/tcp-activation.md)。  
  
### <a name="to-create-a-basic-service-hosted-by-was"></a>创建 WAS 承载的基本服务  
  
1. 为该类型的服务定义服务协定。  
  
     [!code-csharp[C_HowTo_HostInWAS#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/service.cs#1121)]  
  
2. 在服务类中实现该服务协定。 请注意，在服务的实现内部，未指定地址或绑定信息。 而且，不必编写代码也可从配置文件中检索该信息。  
  
     [!code-csharp[C_HowTo_HostInWAS#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/service.cs#1122)]  
  
3. 创建 Web.config 文件，以定义要由 <xref:System.ServiceModel.NetTcpBinding> 终结点使用的 `CalculatorService` 绑定。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <bindings>  
          <netTcpBinding>  
            <binding portSharingEnabled="true">  
              <security mode="None" />  
            </binding>  
          </netTcpBinding>  
        </bindings>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
4. 创建包含以下代码的 Service.svc 文件。  
  
   ```aspx-csharp
   <%@ServiceHost language=c# Service="CalculatorService" %>
   ```
  
5. 将 Service.svc 文件放到 IIS 虚拟目录中。  
  
### <a name="to-create-a-client-to-use-the-service"></a>创建要使用服务的客户端  
  
1. 使用) 从命令行中 [ ( 的元数据实用工具工具 ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 从服务元数据生成代码。  
  
    ```console
    Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
    ```  
  
2. 生成的客户端包含 `ICalculator` 接口，该接口定义了客户端实现必须满足的服务协定。  
  
     [!code-csharp[C_HowTo_HostInWAS#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1221)]  
  
3. 生成的客户端应用程序还包含 `ClientCalculator` 的实现。 请注意，在服务的实现内部，未指定地址和绑定信息。 而且，不必编写代码也可从配置文件中检索该信息。  
  
     [!code-csharp[C_HowTo_HostInWAS#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1222)]  
  
4. 使用 <xref:System.ServiceModel.NetTcpBinding> 的客户端配置也通过 Svcutil.exe 生成。 在使用 Visual Studio 时，应在 App.config 文件中命名此文件。  
  
     [!code-xml[C_HowTo_HostInWAS#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/common/app.config#2211)]
  
5. 在应用程序中创建 `ClientCalculator` 的实例，然后调用服务操作。  
  
     [!code-csharp[C_HowTo_HostInWAS#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1223)]  
  
6. 编译并运行客户端。  
  
## <a name="see-also"></a>另请参阅

- [TCP 激活](../samples/tcp-activation.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
