---
title: MTOM 编码
ms.date: 03/30/2017
ms.assetid: 820e316f-4ee1-4eb5-ae38-b6a536e8a14f
ms.openlocfilehash: c2b5cc59f2e2a80a323070e452dfcf7c4c0cc4d6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96260202"
---
# <a name="mtom-encoding"></a>MTOM 编码

此示例演示如何将消息传输优化机制 (MTOM) 消息编码与 WSHttpBinding 一起使用。 MTOM 是一种机制，用来以原始字节形式传输包含 SOAP 消息的较大二进制附件，从而使所传输的消息较小。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\MTOM`  
  
 默认情况下，WSHttpBinding 以正常文本 XML 形式发送和接收消息。 若要允许发送和接收 MTOM 消息，请在绑定的配置中设置 `messageEncoding` 属性 (Attribute)（如下面的示例代码中所示），或者使用 `MessageEncoding` 属性 (Property) 直接在绑定中进行设置。 服务或客户端现在可以发送和接收 MTOM 消息了。  
  
```xml  
<wsHttpBinding>  
  <binding name="WSHttpBinding_IUpload" messageEncoding="Mtom" />  
</wsHttpBinding>  
```  
  
 MTOM 编码器可以优化字节和流的数组。 在下面的示例中，操作使用 `Stream` 参数，因此可以进行优化。  

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
  public interface IUpload  
  {  
      [OperationContract]  
      int Upload(Stream data);  
  }  
```
  
 为该示例选择的协定会将二进制数据传输到服务，并将上载的字节数作为返回值接收。 在安装服务之后运行客户端时，服务会显示数字 1000，这表示收到了全部 1000 个字节。 剩下的输出列出了在各种负载情况下经过优化和未经优化的消息大小。  
  
```console
Output:  
1000  
  
Text encoding with a 100 byte payload: 433  
MTOM encoding with a 100 byte payload: 912  
  
Text encoding with a 1000 byte payload: 1633  
MTOM encoding with a 1000 byte payload: 2080  
  
Text encoding with a 10000 byte payload: 13633  
MTOM encoding with a 10000 byte payload: 11080  
  
Text encoding with a 100000 byte payload: 133633  
MTOM encoding with a 100000 byte payload: 101080  
  
Text encoding with a 1000000 byte payload: 1333633  
MTOM encoding with a 1000000 byte payload: 1001080  
  
Press <ENTER> to terminate client.  
```  
  
 使用 MTOM 的目的在于优化对较大的二进制负载的传输。 对于较小的二进制负载来说，使用 MTOM 发送 SOAP 消息会产生显著的开销，但是，当这些负载增大到几千个字节时，该开销会变得微不足道。 原因在于，正常文本 XML 使用 Base64 对二进制数据进行编码，这要求每三个字节对应四个字符，从而使得数据的大小增加三分之一。 MTOM 能够以原始字节形式传输二进制数据，这会缩短编码/解码时间并生成较小的消息。 与当今的业务文档和数字照片相比，几千个字节的阈值是非常小的。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 使用以下命令安装 ASP.NET 4.0。  
  
    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
