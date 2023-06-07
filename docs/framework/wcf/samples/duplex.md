---
title: 双工
ms.date: 03/30/2017
helpviewer_keywords:
- Duplex Service Contract
ms.assetid: bc5de6b6-1a63-42a3-919a-67d21bae24e0
ms.openlocfilehash: 77eab6a4975fc67c20558a53f399c7e709215587
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575194"
---
# <a name="duplex"></a>双工

“双工”示例演示如何定义和实现双工协定。 当客户端与服务建立会话并为服务提供可用来将消息发送回客户端的通道时，就会发生双工通信。 此示例基于[入门](getting-started-sample.md)。 双工协定以一对接口形式定义：一个从客户端到服务的主接口和一个从服务到客户端的回调接口。 在本示例中，`ICalculatorDuplex` 接口允许客户端执行数学运算，通过会话计算结果。 服务在 `ICalculatorDuplexCallback` 接口上返回结果。 双工协定需要会话，因为必须建立上下文才能将客户端和服务之间发送的一组消息关联在一起。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

在此示例中，客户端是一个控制台应用程序 (.exe)，服务是由 Internet 信息服务 (IIS) 承载的。 双工协定定义如下：

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples", SessionMode=SessionMode.Required,
                 CallbackContract=typeof(ICalculatorDuplexCallback))]
public interface ICalculatorDuplex
{
    [OperationContract(IsOneWay = true)]
    void Clear();
    [OperationContract(IsOneWay = true)]
    void AddTo(double n);
    [OperationContract(IsOneWay = true)]
    void SubtractFrom(double n);
    [OperationContract(IsOneWay = true)]
    void MultiplyBy(double n);
    [OperationContract(IsOneWay = true)]
    void DivideBy(double n);
}

public interface ICalculatorDuplexCallback
{
    [OperationContract(IsOneWay = true)]
    void Result(double result);
    [OperationContract(IsOneWay = true)]
    void Equation(string eqn);
}
```

`CalculatorService` 类实现 `ICalculatorDuplex` 主接口。 服务使用 <xref:System.ServiceModel.InstanceContextMode.PerSession> 实例模式来维护每个会话的结果。 名为 `Callback` 的私有属性用于访问指向客户端的回调通道。 服务使用该回调通过回调接口将消息发送回客户端。

```csharp
[ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]
public class CalculatorService : ICalculatorDuplex
{
    double result = 0.0D;
    string equation;

    public CalculatorService()
    {
        equation = result.ToString();
    }

    public void Clear()
    {
        Callback.Equation($"{equation} = {result}");
        equation = result.ToString();
    }

    public void AddTo(double n)
    {
        result += n;
        equation += $" + {n}";
        Callback.Result(result);
    }

    //...

    ICalculatorDuplexCallback Callback
    {
        get
        {
            return OperationContext.Current.GetCallbackChannel<ICalculatorDuplexCallback>();
        }
    }
}
```

客户端必须提供一个实现双工协定回调接口的类，以便从服务接收消息。 该示例中，定义 `CallbackHandler` 类以实现 `ICalculatorDuplexCallback` 接口。

```csharp
public class CallbackHandler : ICalculatorDuplexCallback
{
   public void Result(double result)
   {
      Console.WriteLine("Result({0})", result);
   }

   public void Equation(string equation)
   {
      Console.WriteLine("Equation({0}", equation);
   }
}
```

针对双工协定生成的代理需要在构造时提供一个 <xref:System.ServiceModel.InstanceContext>。 此 <xref:System.ServiceModel.InstanceContext> 用作实现回调接口并处理从服务发送回的消息的对象所在的位置。 <xref:System.ServiceModel.InstanceContext> 是用 `CallbackHandler` 类的实例构造的。 此对象处理通过回调接口从服务发送到客户端的消息。

```csharp
// Construct InstanceContext to handle messages on callback interface.
InstanceContext instanceContext = new InstanceContext(new CallbackHandler());

// Create a client.
CalculatorDuplexClient client = new CalculatorDuplexClient(instanceContext);

Console.WriteLine("Press <ENTER> to terminate client once the output is displayed.");
Console.WriteLine();

// Call the AddTo service operation.
double value = 100.00D;
client.AddTo(value);

// Call the SubtractFrom service operation.
value = 50.00D;
client.SubtractFrom(value);

// Call the MultiplyBy service operation.
value = 17.65D;
client.MultiplyBy(value);

// Call the DivideBy service operation.
value = 2.00D;
client.DivideBy(value);

// Complete equation.
client.Clear();

Console.ReadLine();

//Closing the client gracefully closes the connection and cleans up resources.
client.Close();
```

已将配置修改为提供同时支持会话通信和双工通信的绑定。 `wsDualHttpBinding` 支持会话通信，并通过提供双 HTTP 连接（一个连接对应于一个方向）来允许进行双工通信。 在服务上，配置中的唯一区别是所用的绑定。 在客户端上，必须配置一个服务器可用来连接客户端的地址，如下面的示例配置所示。

```xml
<client>
  <endpoint name=""
            address="http://localhost/servicemodelsamples/service.svc"
            binding="wsDualHttpBinding"
            bindingConfiguration="DuplexBinding"
            contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
</client>

<bindings>
  <!-- Configure a binding that support duplex communication. -->
  <wsDualHttpBinding>
    <binding name="DuplexBinding"
             clientBaseAddress="http://localhost:8000/myClient/">
    </binding>
  </wsDualHttpBinding>
</bindings>
```

当运行示例时，从服务发送的回调接口上，可以看到返回到客户端的消息。 显示每个中间结果，最后在所有操作完成时显示整个公式。 按 Enter 可关闭客户端。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 c #、c + + 或 Visual Basic .NET 版本的解决方案，请按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

    > [!IMPORTANT]
    > 在跨计算机配置中运行客户端时，请确保 `address` 使用相应计算机的名称，将元素的属性[ \<endpoint> \<client> ](../../configure-apps/file-schema/wcf/endpoint-of-client.md)中的 "localhost" 替换为该元素的元素的属性 `clientBaseAddress` ，如下 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md) 所示：

    ```xml
    <client>
        <endpoint name = ""
        address="http://service_machine_name/servicemodelsamples/service.svc"
        ... />
    </client>
    ...
    <wsDualHttpBinding>
        <binding name="DuplexBinding" clientBaseAddress="http://client_machine_name:8000/myClient/">
        </binding>
    </wsDualHttpBinding>
    ```

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Duplex`
