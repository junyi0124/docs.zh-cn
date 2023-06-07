---
title: 使用传输安全的 BasicBinding
ms.date: 03/30/2017
ms.assetid: f49b1de6-0254-4362-8ef2-fccd8ff9688b
ms.openlocfilehash: 822a7dcb20c6559a70ba77719b6e7a62633bb31c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575662"
---
# <a name="basicbinding-with-transport-security"></a>使用传输安全的 BasicBinding

本示例演示对基本绑定使用 SSL 传输安全。 此示例基于实现计算器服务的[入门](getting-started-sample.md)。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Basic\TransportSecurity`

## <a name="sample-details"></a>示例详细信息

默认情况下，基本绑定支持 HTTP 通信。 本示例演示如何为基本绑定启用传输安全。 在运行此示例之前，必须通过使用 Web 服务器证书向导来创建证书并分配此证书。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

示例中的程序代码与[入门](getting-started-sample.md)服务的程序代码相同。 配置文件设置中的终结点定义和绑定定义已进行修改，可以启用安全通信，如下面的示例配置所示。

```xml
<system.serviceModel>
  <services>
    <service type="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <endpoint address=""
                binding="basicHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
   </services>
  <bindings>
    <basicHttpBinding>
      <!-- Configure basicHttpBinding with Transport security -->
      <!-- mode and clientCredentialType set to None. -->
      <binding name="Binding1">
        <security mode="Transport">
          <transport clientCredentialType="None"/>
        </security>
      </binding>
    </basicHttpBinding>
  </bindings>
</system.serviceModel>
```

由于本示例中使用的证书是用 Makecert 创建的测试证书，因此，当你尝试在浏览器中访问 HTTPS：地址（例如）时，将出现安全 `https://localhost/servicemodelsamples/service.svc` 警报。 若要允许 Windows Communication Foundation （WCF）客户端使用测试证书，则会向客户端添加一些额外的代码以禁止显示安全警报。 使用真正的证书时不需要此代码和附带的类。

```csharp
// This code is required only for test certificates such as those
// created by Makecert.exe.
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

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 使用以下命令安装 ASP.NET 4.0：

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

3. 确保已执行[Internet Information Services （IIS）服务器证书安装说明](iis-server-certificate-installation-instructions.md)。

4. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

5. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。
