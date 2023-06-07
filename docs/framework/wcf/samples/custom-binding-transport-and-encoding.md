---
title: 自定义绑定传输和编码
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6c0b353d-79ee-4e61-b348-be49ad0e9a16
ms.openlocfilehash: 00af245f49994314ca29e1f5a2debe3af2364999
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241858"
---
# <a name="custom-binding-transport-and-encoding"></a>自定义绑定传输和编码

自定义绑定由离散绑定元素的有序列表定义。 本示例演示如何使用各种传输和消息编码元素配置自定义绑定。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 此示例基于 [自宿主](self-host.md)，已修改为配置三个终结点，以支持具有自定义绑定的 HTTP、TCP 和 NamedPipe 传输。 并对客户端配置进行了类似修改，将客户端代码更改为能与这三个终结点中的每一个进行通信。  
  
 本示例演示如何配置支持特定传输和消息编码的自定义绑定。 这是通过为 `binding` 元素配置传输和消息编码而实现的。 在定义自定义绑定时，绑定元素的排序非常重要，因为每个元素都表示通道堆栈中的一个层 (请参阅 [自定义绑定](../extending/custom-bindings.md)) 。 本示例配置三个自定义绑定：使用文本编码的 HTTP 传输、使用文本编码的 TCP 传输和使用二进制编码的 NamedPipe 传输。  
  
 服务配置按如下所示定义自定义绑定：  
  
```xml  
<bindings>  
    <customBinding>  
        <binding name="HttpBinding" >  
            <textMessageEncoding
                messageVersion="Soap12Addressing10"/>  
            <httpTransport />  
        </binding>  
        <binding name="TcpBinding" >  
            <textMessageEncoding />  
            <tcpTransport />  
        </binding>  
        <binding name="NamedPipeBinding" >  
            <binaryMessageEncoding />  
            <namedPipeTransport />  
        </binding>  
    </customBinding>  
</bindings>  
```  
  
 运行示例时，操作请求和响应将显示在服务和客户端控制台窗口中。 客户端将逐一与三个终结点进行通信，首先访问 HTTP，接着是 TCP，最后为 NamedPipe。 在每个控制台窗口中按 Enter 可以关闭服务和客户端。  
  
 `namedPipeTransport` 绑定不支持计算机到计算机的操作。 它仅用于同一计算机上的通信。 因此，当在跨计算机的方案中运行示例时，应注释掉客户端代码文件中的以下行：  
  
```csharp  
CalculatorClient client = new CalculatorClient("default");  
Console.WriteLine("Communicate with named pipe endpoint.");  
// Call operations.  
DoCalculations(client);  
//Closing the client gracefully closes the connection and cleans up resources  
client.Close();  
```  
  
```vb  
Dim client As New CalculatorClient("default")  
Console.WriteLine("Communicate with named pipe endpoint.")  
' call operations  
DoCalculations(client)  
'Closing the client gracefully closes the connection and cleans up resources  
client.Close()  
```  
  
> [!NOTE]
> 如果使用 Svcutil.exe 为此示例重新生成配置，请确保在客户端配置中修改终结点名称以与客户端代码匹配。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 c #、c + + 或 Visual Basic .NET 版本的解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Custom\Transport`  
