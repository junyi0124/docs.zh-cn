---
title: 错误协定
ms.date: 03/30/2017
ms.assetid: b31b140e-dc3b-408b-b3c7-10b6fe769725
ms.openlocfilehash: 898692119e3e71b1c5aeedcd65674a49842ef110
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245206"
---
# <a name="fault-contract"></a>错误协定

“错误协定”示例演示如何将错误信息从服务传达到客户端。 该示例基于 [入门](getting-started-sample.md)，并向服务添加了一些附加代码，以将内部异常转换为错误。 客户端试图执行除数为零的运算以在服务上强制产生错误情况。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 已将计算器协定修改为包括 <xref:System.ServiceModel.FaultContractAttribute>，如下面的示例代码所示。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    int Add(int n1, int n2);  
    [OperationContract]  
    int Subtract(int n1, int n2);  
    [OperationContract]  
    int Multiply(int n1, int n2);  
    [OperationContract]  
    [FaultContract(typeof(MathFault))]  
    int Divide(int n1, int n2);  
}  
```  
  
 <xref:System.ServiceModel.FaultContractAttribute> 属性指示 `Divide` 运算可能返回一个 `MathFault` 类型的错误。 错误可以是可序列化的任何类型。 在本例中，`MathFault` 为数据协定，如下所示：  
  
```csharp
[DataContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public class MathFault  
{
    private string operation;  
    private string problemType;  
  
    [DataMember]  
    public string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [DataMember]
    public string ProblemType  
    {  
        get { return problemType; }  
        set { problemType = value; }  
    }  
}  
```  
  
 如下面的示例代码所示，发生除数为零异常时，`Divide` 方法引发 <xref:System.ServiceModel.FaultException%601> 异常。 此异常导致向客户端发送一个错误。  
  
```csharp
public int Divide(int n1, int n2)  
{  
    try  
    {  
        return n1 / n2;  
    }  
    catch (DivideByZeroException)  
    {  
        MathFault mf = new MathFault();  
        mf.operation = "division";  
        mf.problemType = "divide by zero";  
        throw new FaultException<MathFault>(mf);  
    }  
}  
```  
  
 通过请求除数为零，客户端代码强制产生错误。 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 您会看到将除数为零报告为错误。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Add(15,3) = 18  
Subtract(145,76) = 69  
Multiply(9,81) = 729  
FaultException<MathFault>: Math fault while doing division. Problem: divide by zero  
  
Press <ENTER> to terminate client.  
```  
  
 客户端通过捕获相应 `FaultException<MathFault>` 异常来完成此操作：  
  
```csharp
catch (FaultException<MathFault> e)  
{  
    Console.WriteLine("FaultException<MathFault>: Math fault while doing " + e.Detail.operation + ". Problem: " + e.Detail.problemType);  
    client.Abort();  
}  
```  
  
 默认情况下，不可预期的异常的详细信息不发送到客户端，以防止服务实现的详细信息泄露出服务的安全边界。 `FaultContract` 提供一种方法来描述协定中的故障并将某些类型的异常标记为适合传输到客户端。 `FaultException<T>` 提供了用于向使用者发送故障的运行时机制。  
  
 但是，在调试时查看服务失败的内部详细信息很有用。 若要关闭上述安全行为，您可以指示在发送到客户端的错误中必须包括服务器上每个未处理的异常的详细信息。 这是通过将 <xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A> 设置为 `true` 来完成的。 你可以在代码中或在配置中设置它，如下面的示例所示。  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="True" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 此外，通过将 `behaviorConfiguration` 配置文件中服务的属性设置为 "CalculatorServiceBehavior"，此行为必须与服务相关联。  
  
 若要在客户端上捕获这样的错误，必须捕获非泛型 <xref:System.ServiceModel.FaultException>。  
  
 此行为仅用于调试目的，切勿在生产中启用它。  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Faults`  
