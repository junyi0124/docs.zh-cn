---
title: 默认消息协定
ms.date: 03/30/2017
helpviewer_keywords:
- Message Contract
ms.assetid: 5a200b78-1a46-4104-b7fb-da6dbab33893
ms.openlocfilehash: 0a70fb519f3b3e8d9ce109c3b7bef0313e22eb50
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292813"
---
# <a name="default-message-contract"></a>默认消息协定

默认消息协定示例演示了一个服务，在该服务中，用户定义的自定义消息会在服务操作来回传递。 此示例基于将计算器接口实现为类型化服务的 [入门](getting-started-sample.md) 。 此示例不是 [入门](getting-started-sample.md)中使用的加法、减法、乘法和除法的单个服务操作，而是传递包含操作数和运算符的自定义消息，并返回算术计算的结果。  
  
 客户端是一种控制台程序 (.exe)，服务库 (.dll) 是由 Internet 信息服务 (IIS) 承载的。 客户端活动显示在控制台窗口中。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 在服务中，定义了单个服务操作，该操作接受和返回 `MyMessage` 类型的自定义消息。 在该示例中，尽管请求消息和响应消息属于同一种类型，但必要时当然也可以属于不同的消息协定。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract(Action="http://test/MyMessage_action",  
                  ReplyAction="http://test/MyMessage_action")]  
    MyMessage Calculate(MyMessage request);  
}  
```  
  
 自定义消息 `MyMessage` 是在用 <xref:System.ServiceModel.MessageContractAttribute>、<xref:System.ServiceModel.MessageHeaderAttribute> 和 <xref:System.ServiceModel.MessageBodyMemberAttribute> 属性进行批注的类中定义的。 本示例中只使用第三个构造函数。 使用消息协定，您可以对 SOAP 消息进行完全控制。 在本示例中，<xref:System.ServiceModel.MessageHeaderAttribute> 属性用来将 `Operation` 放在 SOAP 头中。 操作数 `N1` 和 `N2` 以及 `Result` 出现在 SOAP 正文中，因为它们应用了 <xref:System.ServiceModel.MessageBodyMemberAttribute> 属性。  
  
```csharp
[MessageContract]  
public class MyMessage  
{  
    private string operation;  
    private double n1;  
    private double n2;  
    private double result;  
  
    //Constructor - create an empty message.  
  
    public MyMessage() {}  
  
    //Constructor - create a message and populate its members.  
  
    public MyMessage(double n1, double n2, string operation,
                     double result)  
    {  
        this.n1 = n1;  
        this.n2 = n2;  
        this.operation = operation;  
        this.result = result;  
    }  
  
    //Constructor - create a message from another message.  
  
    public MyMessage(MyMessage message)  
    {  
        this.n1 = message.n1;  
        this.n2 = message.n2;  
        this.operation = message.operation;  
        this.result = message.result;  
    }  
  
    [MessageHeader]  
    public string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [MessageBodyMember]  
    public double N1  
    {  
        get { return n1; }  
        set { n1 = value; }  
    }  
  
    [MessageBodyMember]  
    public double N2  
    {  
        get { return n2; }  
        set { n2 = value; }  
    }  
  
    [MessageBodyMember]  
    public double Result  
    {  
        get { return result; }  
        set { result = value; }  
    }  
}  
```  
  
 实现类包含 `Calculate` 服务操作的代码。 `CalculateService` 类从请求消息获取操作数和运算符，并创建一条包含所请求的计算的结果的响应消息，如下面的示例代码所示。  
  
```csharp
// Service class which implements the service contract.  
public class CalculatorService : ICalculator  
{  
    // Perform a calculation.  
  
    public MyMessage Calculate(MyMessage request)  
    {  
        MyMessage response = new MyMessage(request);  
        switch (request.Operation)  
        {  
            case "+":  
                response.Result = request.N1 + request.N2;  
                break;  
            case "-":  
                response.Result = request.N1 - request.N2;  
                break;  
            case "*":  
                response.Result = request.N1 * request.N2;  
                break;  
            case "/":  
                response.Result = request.N1 / request.N2;  
                break;  
            default:  
                response.Result = 0.0D;  
                break;  
        }  
        return response;  
    }  
}  
```  
  
 为客户端生成的客户端代码是通过 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 工具创建的。 在必要时，该工具会自动在所生成的客户端代码中创建消息协定类型。 可以指定 `/messageContract` 命令选项来强制生成消息协定。  
  
```console  
svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" /o:client\generatedClient.cs http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 下面的示例代码演示使用 `MyMessage` 消息的客户端。  
  
```csharp
// Create a client with given client endpoint configuration  
CalculatorClient client = new CalculatorClient();  
  
// Perform addition using a typed message.  
  
MyMessage request = new MyMessage()
                    {  
                        N1 = 100D,  
                        N2 = 15.99D,  
                        Operation = "+"  
                    };
MyMessage response = ((ICalculator)client).Calculate(request);  
Console.WriteLine("Add({0},{1}) = {2}", request.N1, request.N2, response.Result);  
```  
  
 运行示例时，计算结果将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 此时，用户定义的自定义消息已经在客户端和服务操作之间传递。 消息协定进行了如下定义：操作数和结果位于消息正文中，运算符位于消息头中。 消息日志记录可以配置为遵守这个消息结构。  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Message\Default`  
