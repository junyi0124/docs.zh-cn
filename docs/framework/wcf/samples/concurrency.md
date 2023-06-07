---
title: 并发
ms.date: 03/30/2017
helpviewer_keywords:
- service behaviors, concurency sample
- Concurrency Sample [Windows Communication Foundation]
ms.assetid: f8dbdfb3-6858-4f95-abe3-3a1db7878926
ms.openlocfilehash: 69692f48cc1f45057e865a3908ddf41afc599bb1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243246"
---
# <a name="concurrency"></a>并发

“并发”示例演示如何使用具有 <xref:System.ServiceModel.ServiceBehaviorAttribute> 枚举的 <xref:System.ServiceModel.ConcurrencyMode>，该枚举控制服务的实例是依次还是同时处理消息。 该示例基于实现服务协定的 [入门](getting-started-sample.md) `ICalculator` 。 本示例定义从 `ICalculatorConcurrency` 中继承的新协定 `ICalculator`，该协定提供两个用于检查服务并发状态的附加操作。 通过更改并发设置，可以在运行客户端时观察到行为发生的变化。  
  
 在此示例中，客户端是一个控制台应用程序 (.exe)，服务是由 Internet 信息服务 (IIS) 承载的。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 有三种可用的并发模式：  
  
- `Single`：每个服务实例一次处理一个消息。 这是默认的并发模式。  
  
- `Multiple`：每个服务实例同时处理多个消息。 若要使用此并发模式，服务实现必须是线程安全的。  
  
- `Reentrant`：每个服务实例一次处理一个消息，但接受可重入调用。 服务仅在调用时接受这些调用。ConcurrencyMode 示例演示了可重入[。](concurrencymode-reentrant.md)  
  
 并发的使用与实例化模式有关。 在 <xref:System.ServiceModel.InstanceContextMode.PerCall> 实例化过程中，与并发没有关系，因为每个消息都由一个新服务实例处理。 在 <xref:System.ServiceModel.InstanceContextMode.Single> 实例化过程中，与 <xref:System.ServiceModel.ConcurrencyMode.Single> 或 <xref:System.ServiceModel.ConcurrencyMode.Multiple> 并发有关，具体取决于单个实例是依次还是同时处理消息。 在 <xref:System.ServiceModel.InstanceContextMode.PerSession> 实例化过程中，可能与任何并发模式有关。  
  
 服务类使用 `[ServiceBehavior(ConcurrencyMode=<setting>)]` 属性来指定并发行为，如下面的代码示例所示。 通过更换注释掉的行，您可以体验 `Single` 和 `Multiple` 并发模式。 请记住，更改并发模式之后重新生成服务。  
  
```csharp
// Single allows a single message to be processed sequentially by each service instance.  
//[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Single, InstanceContextMode = InstanceContextMode.Single)]  
  
// Multiple allows concurrent processing of multiple messages by a service instance.  
// The service implementation should be thread-safe. This can be used to increase throughput.  
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple, InstanceContextMode = InstanceContextMode.Single)]  
  
// Uses Thread.Sleep to vary the execution time of each operation.  
public class CalculatorService : ICalculatorConcurrency  
{  
    int operationCount;  
  
    public double Add(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(180);  
        return n1 + n2;  
    }  
  
    public double Subtract(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(100);  
        return n1 - n2;  
    }  
  
    public double Multiply(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(150);  
        return n1 * n2;  
    }  
  
    public double Divide(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(120);  
        return n1 / n2;  
    }  
  
    public string GetConcurrencyMode()  
    {
        // Return the ConcurrencyMode of the service.  
        ServiceHost host = (ServiceHost)OperationContext.Current.Host;  
        ServiceBehaviorAttribute behavior = host.Description.Behaviors.Find<ServiceBehaviorAttribute>();  
        return behavior.ConcurrencyMode.ToString();  
    }  
  
    public int GetOperationCount()  
    {
        // Return the number of operations.  
        return operationCount;  
    }  
}  
```  
  
 默认情况下，此示例使用通过 <xref:System.ServiceModel.ConcurrencyMode.Multiple> 实例化的 <xref:System.ServiceModel.InstanceContextMode.Single> 并发。 已经对客户端代码进行了修改，以便使用异步代理。 这允许客户端对服务进行多个调用，而不必在每个调用之间的等待响应。 您可以观察到服务并发模式的行为中的差异。  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 显示运行服务所用的并发模式、调用每个操作，然后显示操作计数。 请注意，并发模式为 `Multiple` 时，返回结果的顺序与调用的顺序不同，因为服务同时处理多个消息。 通过将并发模式更改为 `Single`，可以按调用顺序返回结果，因为服务按顺序处理每个消息。 在客户端窗口中按 Enter 可以关闭客户端。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 如果使用 Svcutil.exe 生成代理客户端，请确保包含 `/async` 选项。  
  
3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Behaviors\Concurrency`  
