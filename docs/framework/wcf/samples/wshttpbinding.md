---
title: WSHttpBinding
ms.date: 03/30/2017
helpviewer_keywords:
- WS Profile binding
ms.assetid: 22d85b19-0135-4141-9179-a0e9c343ad73
ms.openlocfilehash: 6e5946dd7d107c34eafe55a62c51d089931b5b77
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252277"
---
# <a name="wshttpbinding"></a>WSHttpBinding

此示例演示如何使用 Windows Communication Foundation (WCF) 实现典型的服务和典型的客户端。 此示例由客户端控制台程序 (client.exe) 和 Internet 信息服务 (IIS) 所承载的服务库组成。 该服务实现定义“请求-答复”通信模式的协定。 该协定由 `ICalculator` 接口定义，此接口公开数学运算（加、减、乘和除）。 客户端向给定的数学运算发出同步请求，服务使用结果进行回复。 客户端活动显示在控制台窗口中。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\wsHttp`  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 此示例使用公开 `ICalculator` 协定 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 。 此绑定的配置已在 Web.config 文件中展开。  
  
```xml
<bindings>  
  <wsHttpBinding>  
    <!--The following is the expanded configuration section for a-->  
    <!--WSHttpBinding. Each property is configured with the default-->
    <!--value. See the ReliableSession, TransactionFlow, -->  
    <!--TransportSecurity, and MessageSecurity samples in the WS -->  
    <!--directory to learn how to configure these features. -->  
    <binding name="Binding1"  
              bypassProxyOnLocal="false"
              transactionFlow="false"
              hostNameComparisonMode="StrongWildcard"  
              maxBufferPoolSize="524288"
              maxReceivedMessageSize="65536"  
              messageEncoding="Text"
              textEncoding="utf-8"
              useDefaultWebProxy="true"  
              allowCookies="false">  
      <reliableSession ordered="true"
                       inactivityTimeout="00:10:00"  
                       enabled="false" />  
      <security mode="Message">  
        <message clientCredentialType="Windows"
                 negotiateServiceCredential="true"  
                 algorithmSuite="Default"
                 establishSecurityContext="true" />  
      </security>  
    </binding>  
  </wsHttpBinding>  
</bindings>  
```  
  
 在基 `binding` 元素上，通过 `maxReceivedMessageSize` 值可以配置传入消息的最大大小（以字节为单位）。 通过 `hostNameComparisonMode` 值，可以配置向服务多路分解消息时是否考虑主机名。 通过 `messageEncoding` 值可以配置是对消息使用文本还是 MTOM 编码。 通过 `textEncoding` 值可以为消息配置字符编码。 通过 `bypassProxyOnLocal` 值可以配置是否对本地通信使用 HTTP 协议。 通过 `transactionFlow` 值可以配置是否对当前事务进行流处理（如果为事务流配置了操作）。  
  
 在 [\<reliableSession>](../../configure-apps/file-schema/wcf/reliablesession.md) 元素上，启用的布尔值配置是否启用可靠会话。 `ordered` 值配置是否保留消息顺序。 `inactivityTimeout` 值配置会话在出错之前可以闲置的时间。  
  
 在上 [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) ， `mode` 值配置应该使用的安全模式。 在此示例中，将使用消息安全性，这就是在 [\<message>](../../configure-apps/file-schema/wcf/message-of-wshttpbinding.md) 中指定的原因 [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) 。  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 使用以下命令安装 ASP.NET 4.0。  
  
    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
