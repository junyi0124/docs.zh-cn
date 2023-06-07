---
title: 入门示例
description: 了解如何使用 WCF 实现典型的服务和典型的客户端。 此示例是所有其他基本技术示例的基础。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- basic samples [WCF], getting started
ms.assetid: 967a3d94-0261-49ff-b85a-20bb07f1af20
ms.openlocfilehash: b23be1b33f227154b916429c063ec4106229bb3c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246228"
---
# <a name="getting-started-sample"></a>入门示例

入门示例演示如何使用 Windows Communication Foundation （WCF）实现典型的服务和典型的客户端。 此示例是所有其他基本技术示例的基础。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\GettingStarted\GettingStarted`

服务描述它在服务协定中执行的操作，服务协定由服务作为元数据公开。 服务中还包含用来实现操作的代码。

客户端中包含服务协定的定义，以及一个用来访问服务的代理类。 代理代码是使用 "服务元[数据" 实用工具（Svcutil.exe）](../servicemodel-metadata-utility-tool-svcutil-exe.md)从服务元数据生成的。

在 Windows Vista 上，该服务承载于 Windows 激活服务（WAS）中。 在 Windows XP 和 Windows Server 2003 上，它是由 Internet Information Services （IIS）和 ASP.NET 承载的。 如果将服务承载于 IIS 或 WAS 中，那么，在首次访问服务时，系统将自动激活服务。

> [!NOTE]
> 如果希望开始使用在控制台应用程序（而非 IIS）中承载服务的示例，请参阅[自承载](self-host.md)示例。

服务和客户端的配置文件设置中均指定了访问详细信息，这些设置在部署时提供了灵活性。 其中包括指定地址、绑定和协定的终结点定义。 绑定为如何访问服务指定了传输和安全详细信息。

服务配置了一个运行时行为来发布其元数据。

该服务实现定义“请求-答复”通信模式的协定。 该协定由 `ICalculator` 接口定义，此接口公开数学运算（加、减、乘和除）。 客户端向给定的数学运算发出请求，服务使用结果进行回复。 服务实现一个 `ICalculator` 协定，下面的代码对该协定进行了定义。

```vb
' Define a service contract.
    <ServiceContract(Namespace:="http://Microsoft.Samples.GettingStarted")>
     Public Interface ICalculator
        <OperationContract()>
        Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double
    End Interface
```

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
}
```

服务实现计算并返回相应的结果，如下面的示例代码中所示。

```vb
' Service class which implements the service contract.
Public Class CalculatorService
Implements ICalculator
Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Add
Return n1 + n2
End Function

Public Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Subtract
Return n1 - n2
End Function

Public Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Multiply
Return n1 * n2
End Function

Public Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Divide
Return n1 / n2
End Function
End Class
```

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

服务会公开一个终结点，以便与使用配置文件 (Web.config) 定义的服务进行通信，如下面的示例配置中所示。

```xaml
<services>
    <service
        name="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
        <!-- ICalculator is exposed at the base address provided by
         host: http://localhost/servicemodelsamples/service.svc.  -->
       <endpoint address=""
              binding="wsHttpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
       ...
    </service>
</services>
```

服务在 IIS 或 WAS 主机所提供的基址处公开该终结点。 绑定是用标准 <xref:System.ServiceModel.WSHttpBinding> 进行配置的，该标准配置提供 HTTP 通信以及用来进行寻址和实现安全性的标准 Web 服务协议。 协定是由服务实现的 `ICalculator`。

在配置后， `http://localhost/servicemodelsamples/service.svc` 同一台计算机上的客户端可以访问该服务。 若要使远程计算机上的客户端能够访问该服务，必须指定完全限定域名，而不是本地主机。

默认情况下，框架不公开任何元数据。 因此，该服务将打开 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> ，并在处公开元数据交换（MEX）终结点 `http://localhost/servicemodelsamples/service.svc/mex` 。 下面的配置对此进行了演示。

```xaml
<system.serviceModel>
  <services>
    <service
        name="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
      ...
      <!-- the mex endpoint is exposed at
       http://localhost/servicemodelsamples/service.svc/mex -->
      <endpoint address="mex"
                binding="mexHttpBinding"
                contract="IMetadataExchange" />
    </service>
  </services>

  <!--For debugging purposes set the includeExceptionDetailInFaults
   attribute to true-->
  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceMetadata httpGetEnabled="True"/>
        <serviceDebug includeExceptionDetailInFaults="False" />
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```

客户端使用一种给定的协定类型进行通信，该客户端类由一种由 "工作[（Svcutil.exe）](../servicemodel-metadata-utility-tool-svcutil-exe.md)" 生成的客户端类。 所生成的这个客户端类包含在 generatedClient.cs 文件或 generatedClient.vb 文件中。 此实用工具检索给定服务的元数据，并生成要由客户端应用程序使用给定的协定类型进行通信的客户端。 承载服务必须可用于生成客户端代码，因为将使用该服务来检索更新的元数据。

 在客户端目录中通过从 SDK 命令提示运行以下命令来生成类型化代理：

```console
svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /out:generatedClient.cs
```

若要在 Visual Basic 中生成客户端，请从 SDK 命令提示中键入以下命令：

`Svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /l:vb /out:generatedClient.vb`

通过使用所生成的客户端类，客户端可以通过配置相应的地址和绑定来访问给定的服务终结点。 像服务一样，客户端使用配置文件 (App.config) 来指定要与其通信的终结点。 客户端终结点配置由服务终结点的绝对地址、绑定和协定组成，如下面的示例中所示。

```xaml
<client>
     <endpoint
         address="http://localhost/servicemodelsamples/service.svc"
         binding="wsHttpBinding"
         contract=" Microsoft.ServiceModel.Samples.ICalculator" />
</client>
```

该客户端实现将实例化客户端，并使用类型化接口开始与服务通信，如下面的示例代码中所示。

```vb
' Create a client
Dim client As New CalculatorClient()

' Call the Add service operation.
            Dim value1 = 100.0R
            Dim value2 = 15.99R
            Dim result = client.Add(value1, value2)
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)

' Call the Subtract service operation.
value1 = 145.00R
value2 = 76.54R
result = client.Subtract(value1, value2)
Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result)

' Call the Multiply service operation.
value1 = 9.00R
value2 = 81.25R
result = client.Multiply(value1, value2)
Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result)

' Call the Divide service operation.
value1 = 22.00R
value2 = 7.00R
result = client.Divide(value1, value2)
Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result)

'Closing the client gracefully closes the connection and cleans up resources
```

```csharp
// Create a client.
CalculatorClient client = new CalculatorClient();

// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = client.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

// Call the Subtract service operation.
value1 = 145.00D;
value2 = 76.54D;
result = client.Subtract(value1, value2);
Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

// Call the Multiply service operation.
value1 = 9.00D;
value2 = 81.25D;
result = client.Multiply(value1, value2);
Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

// Call the Divide service operation.
value1 = 22.00D;
value2 = 7.00D;
result = client.Divide(value1, value2);
Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);

//Closing the client releases all communication resources.
client.Close();
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```output
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

此入门示例演示了一种创建服务和客户端的标准方法。 本示例中的其他[基本](basic-sample.md)版本用于演示特定产品功能。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

## <a name="see-also"></a>请参阅

- [如何：在托管应用程序中承载 WCF 服务](../how-to-host-a-wcf-service-in-a-managed-application.md)
- [如何：在 IIS 中承载 WCF 服务](../feature-details/how-to-host-a-wcf-service-in-iis.md)
