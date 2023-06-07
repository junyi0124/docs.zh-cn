---
title: WS 双向 Http
ms.date: 03/30/2017
ms.assetid: 9997eba5-29ec-48db-86f3-fa77b241fb1a
ms.openlocfilehash: 4acf2491c242099f6c8b6c9c01dc18e9c99c9934
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589729"
---
# <a name="ws-dual-http"></a>WS 双向 Http

双向 Http 示例演示如何配置 `WSDualHttpBinding` 绑定。 此示例由客户端控制台程序 (.exe) 和 Internet 信息服务 (IIS) 所承载的服务库 (.dll) 组成。 该服务实现双工协定。 该协定由 `ICalculatorDuplex` 接口定义，该接口公开数学运算（加、减、乘和除）。 在本示例中，`ICalculatorDuplex` 接口允许客户端执行数学运算，通过会话计算运行结果。 服务在 `ICalculatorDuplexCallback` 接口上单独返回结果。 双工协定需要会话，因为必须建立上下文才能将客户端和服务之间发送的一组消息关联在一起。 `WSDualHttpBinding` 绑定支持双工通信。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\DualHttp`

若要使用 `WSDualHttpBinding` 配置服务终结点，请在所示的终结点配置中指定绑定。

```xml
<endpoint address=""
         binding="wsDualHttpBinding"
         contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
```

在客户端上，必须配置一个服务器可用来连接客户端的地址，如下面的示例配置所示。

```xml
<system.serviceModel>
  <client>
    <endpoint address=
         "http://localhost/servicemodelsamples/service.svc"
         binding="wsDualHttpBinding"
         bindingConfiguration="Binding1"
         contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
  </client>

  <bindings>
    <!-- Configure a WSDualHttpBinding that supports duplex -->
    <!-- communication. -->
    <wsDualHttpBinding>
      <binding name="Binding1"
               clientBaseAddress="http://localhost:8000/myClient/"
               useDefaultWebProxy="true"
               bypassProxyOnLocal="false">
      </binding>
    </wsDualHttpBinding>
  </bindings>
</system.serviceModel>
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Press <ENTER> to terminate client once the output is displayed.

Result(100)
Result(50)
Result(882.5)
Result(441.25)
Equation(0 + 100 - 50 * 17.65 / 2 = 441.25)
```

当运行示例时，在回调接口上可以看到从服务发送的、返回到客户端的消息。 显示每个中间结果，最后在所有操作完成时显示整个公式。 按 Enter 可关闭客户端。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 使用以下命令安装 ASP.NET 4.0。

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

4. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

    > [!IMPORTANT]
    > 在跨计算机配置中运行客户端时，请确保将 `address` [ \<endpoint> 的 \<client> ](../../configure-apps/file-schema/wcf/endpoint-of-client.md)和元素的元素的属性中的 localhost 替换为 `clientBaseAddress` [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md) 相应计算机的名称，如下所示：

    ```xml
    <client>
        <endpoint name = ""
          address=
         "http://service_machine_name/servicemodelsamples/service.svc"
        />
    </client>
    ...
    <wsDualHttpBinding>
        <binding name="DuplexBinding" clientBaseAddress=
            "http://client_machine_name:8000/myClient/">
        </binding>
    </wsDualHttpBinding>
    ```
