---
title: WS 传输安全
ms.date: 03/30/2017
ms.assetid: 33a20358-5e1b-458a-a6a9-15753bc7b99b
ms.openlocfilehash: 64059a5a09d49f83c9abda5b2f3d1601acf41a3e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252408"
---
# <a name="ws-transport-security"></a>WS 传输安全

此示例演示如何对 <xref:System.ServiceModel.WSHttpBinding> 绑定使用 SSL 传输安全。 默认情况下，`wsHttpBinding` 绑定提供 HTTP 通信。 针对传输安全配置绑定之后，该绑定即支持 HTTPS 通信。 此示例基于实现计算器服务的 [入门](getting-started-sample.md) 。 `wsHttpBinding` 是在客户端和服务的应用程序配置文件中指定和配置的。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\wsTransportSecurity`  
  
 示例中的程序代码与 [入门](getting-started-sample.md) 服务的程序代码相同。 必须在生成和运行示例之前使用 Web 服务器证书向导创建证书并分配此证书。 配置文件设置中的终结点定义和绑定定义可启用 `Transport` 安全模式，如下面的客户端示例配置所示。  
  
```xml  
<system.serviceModel>  
  
    <client>  
      <!-- this endpoint has an https: address -->  
      <endpoint address="https://localhost/servicemodelsamples/service.svc" binding="wsHttpBinding" bindingConfiguration="Binding1" contract="Microsoft.Samples.TransportSecurity.ICalculator"/>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <!-- configure wsHttpbinding with Transport security mode  
                   and clientCredentialType as None -->  
        <binding name="Binding1">  
          <security mode="Transport">  
            <transport clientCredentialType="None"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
  
  </system.serviceModel>  
```  
  
 指定的地址使用 `https://` 方案。 绑定配置将安全模式设置为 `Transport`。 必须在服务的 Web.config 文件中指定相同的安全模式。  
  
 由于本示例中使用的证书是使用 Makecert.exe 创建的测试证书，因此当你尝试从浏览器访问 https：地址（如）时，将出现安全警报 `https://localhost/servicemodelsamples/service.svc` 。 为了允许 Windows Communication Foundation (WCF) 客户端使用测试证书，已向客户端添加了一些附加代码以禁止显示安全警报。 使用生产证书时，不需要此代码和随附的类。  

```csharp
// This code is required only for test certificates like those created by Makecert.exe.  
PermissiveCertificatePolicy.Enact("CN=ServiceModelSamples-HTTPS-Server");  
```

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
  
3. 请确保已执行 [Internet Information Services (IIS) 服务器证书安装说明](iis-server-certificate-installation-instructions.md)。  
  
4. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
5. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
