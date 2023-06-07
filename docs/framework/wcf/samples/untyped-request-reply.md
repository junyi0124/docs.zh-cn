---
title: 非类型化 Request-Reply
ms.date: 03/30/2017
ms.assetid: 0bf0f9d9-7caf-4d3d-8c9e-2d468cca16a5
ms.openlocfilehash: bcd35bcac928397cad57384fdecb55d7e5ad13c3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294975"
---
# <a name="untyped-requestreply"></a>非类型化请求/答复

此示例演示如何定义使用 Message 类的操作协定。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 此示例基于 [入门](getting-started-sample.md)。 服务协定定义一个采用消息类型作为自变量并返回一个消息的操作。 该操作从消息正文收集所有所需数据并计算总和，然后在返回消息中将此总和作为正文进行发送。  
  
```csharp
[OperationContract(Action = CalculatorService.RequestAction, ReplyAction = CalculatorService.ReplyAction)]  
Message ComputeSum(Message request);  
```  
  
 在服务上，该操作检索在输入消息中传递的整数数组，然后计算总和。 为了发送响应消息，该示例创建一个具有适当消息版本和 Action 的新消息，并将计算出的总和添加为此消息的正文。 下面的示例代码对此进行了演示。  
  
```csharp
public Message ComputeSum(Message request)  
{  
    //The body of the message contains a list of numbers which will be
    //read as a int[] using GetBody<T>  
    int result = 0;  
  
    int[] inputs = request.GetBody<int[]>();  
    foreach (int i in inputs)  
    {  
        result += i;  
    }  
  
    Message response = Message.CreateMessage(request.Version,
                                      ReplyAction, result);  
    return response;  
}  
```  
  
 客户端使用由 "工作的 [元数据实用工具" 工具生成的代码 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 创建远程服务的代理。 若要发送请求消息，客户端必须具有视基础通道而定的消息版本。 因此，它创建一个以其创建的代理通道为范围的新 <xref:System.ServiceModel.OperationContextScope>，后者创建一个 <xref:System.ServiceModel.OperationContext>，它在它的 `OutgoingMessageHeaders.MessageVersion` 属性中填充正确的消息版本。 客户端将一个输入数组作为正文传递给请求消息，然后对代理调用 `ComputeSum`。 客户端随后通过访问答复消息的 `GetBody<T>` 方法来检索它传递的输入总和。 下面的示例代码对此进行了演示。  
  
```csharp
using (new OperationContextScope(client.InnerChannel))  
{  
    // Call the Sum service operation.  
    int[] values = { 1, 2, 3, 4, 5 };  
    Message request = Message.CreateMessage(  
        OperationContext.Current.OutgoingMessageHeaders.MessageVersion,
        RequestAction, values);  
    Message reply = client.ComputeSum(request);  
    int response = reply.GetBody<int>();  
  
    Console.WriteLine("Sum of numbers passed (1,2,3,4,5) = {0}",
                                                       response);  
}  
```  
  
 此示例是 Web 承载的示例，因此必须只运行客户端可执行文件。 下面是客户端上的示例输出。  
  
```console  
Prompt>Client.exe  
Sum of numbers passed (1,2,3,4,5) = 15  
  
Press <ENTER> to terminate client.  
```  
  
 此示例是 Web 承载的示例，因此请查看步骤 3 中提供的链接以了解如何生成和运行此示例。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Message\Untyped`  
