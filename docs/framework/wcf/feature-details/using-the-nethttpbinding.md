---
title: 使用 NetHttpBinding
ms.date: 03/30/2017
ms.assetid: fe134acf-ceca-49de-84a9-05a37e3841f1
ms.openlocfilehash: 85a81c353e779800a9aa371658f2f799365b759d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282023"
---
# <a name="using-the-nethttpbinding"></a>使用 NetHttpBinding

<xref:System.ServiceModel.NetHttpBinding> 是为使用 HTTP 或 WebSocket 服务设计的绑定，默认情况下使用二进制编码。 <xref:System.ServiceModel.NetHttpBinding> 将检测它是否与请求-答复协定或双工协定结合使用，并更改其行为以进行匹配 ― 它将针对请求-答复协定使用 HTTP，并针对双工协定使用 WebSocket。 可使用 <xref:System.ServiceModel.Channels.WebSocketTransportUsage> 设置来重写此行为：  
  
1. <xref:System.ServiceModel.Channels.WebSocketTransportUsage.Always> -这会强制使用 Websocket，甚至用于请求-答复协定。  
  
2. <xref:System.ServiceModel.Channels.WebSocketTransportUsage.Never> -这会阻止使用 Websocket。 尝试使用具有此设置的双工协定将导致异常。  
  
3. <xref:System.ServiceModel.Channels.WebSocketTransportUsage.WhenDuplex> -这是默认值，其行为如上所述。  
  
 <xref:System.ServiceModel.NetHttpBinding> 支持 HTTP 模式和 WebSocket 模式下的可靠会话。 在 WebSocket 模式下，会话由传输来提供。  
  
> [!WARNING]
> 在使用 <xref:System.ServiceModel.NetHttpBinding> 且该绑定的 TransferMode 设置为 TransferMode.Streamed 时，较大的流可能会造成死锁，调用将会超时。 若要解决此问题，请发送较小的流，或使用 TransferMode.Buffered。  
  
## <a name="configuring-a-service-to-use-nethttpbinding"></a>将服务配置为使用 NetHttpBinding  

 <xref:System.ServiceModel.NetHttpBinding> 的配置方式与任何其他绑定的配置方式相同。 以下配置代码段说明了如何通过 <xref:System.ServiceModel.NetHttpBinding> 配置 WCF 服务。  
  
```xml  
<system.serviceModel>  
    <services>  
      <service name="WcfService1.Service1">  
        <endpoint address=""  
                  binding="netHttpBinding"  
                  contract="WcfService1.IService1"/>  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange"/>  
      </service>  
    </services>  
    <bindings>  
      <netHttpBinding>  
        <binding name="My_NetHttpBindingConfig">  
          <webSocketSettings transportUsage="WhenDuplex"/>  
        </binding>  
      </netHttpBinding>  
    </bindings>  
    ...
  </system.serviceModel>  
```  
  
 下面的代码段演示如何用代码添加 <xref:System.ServiceModel.NetHttpBinding>。  
  
```csharp  
ServiceHost svchost = new ServiceHost(typeof(Service1), baseAddress);  
            NetHttpBinding binding = new NetHttpBinding();  
            svchost.AddServiceEndpoint(typeof(IService1), binding, address);
        }  
```  
  
## <a name="see-also"></a>另请参阅

- [配置服务绑定](../configuring-bindings-for-wcf-services.md)
- [绑定](bindings.md)
- [系统提供的绑定](../system-provided-bindings.md)
- [双工服务](duplex-services.md)
