---
title: 使用 WCF 客户端访问服务
description: 了解如何为 WCF 服务创建 WCF 客户端代理。 客户端应用程序使用客户端代理与服务进行通信。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- clients [WCF], consuming services
ms.assetid: d780af9f-73c5-42db-9e52-077a5e4de7fe
ms.openlocfilehash: b6f5cd7217b447256f19891c2624fba857735107
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294867"
---
# <a name="accessing-services-using-a-wcf-client"></a>使用 WCF 客户端访问服务

创建服务后，下一步是创建 WCF 客户端代理。 客户端应用程序使用 WCF 客户端代理与服务进行通信。 客户端应用程序通常会导入服务的元数据，以生成可用于调用服务的 WCF 客户端代码。

 创建 WCF 客户端的基本步骤如下所示：

1. 编译服务代码。

2. 生成 WCF 客户端代理。

3. 实例化 WCF 客户端代理。

可以使用服务模型元数据实用工具工具手动生成 WCF 客户端代理 ( # A0) 有关详细信息 [)  ( ](servicemodel-metadata-utility-tool-svcutil-exe.md)，请参阅 "" 还可以使用 **添加服务引用**  功能在 Visual Studio 中生成 WCF 客户端代理。 若要使用上述两种方法中的一种生成 WCF 客户端代理，必须运行该服务。 如果服务是自承载服务，则必须运行主机。 如果服务是在 IIS/WAS 中承载的，则无需执行任何其他操作。

## <a name="servicemodel-metadata-utility-tool"></a>ServiceModel 元数据实用工具

 [ ( # A0) ](servicemodel-metadata-utility-tool-svcutil-exe.md)是一种命令行工具，用于从元数据生成代码。 下面是一个基本 Svcutil.exe 命令示例。

```console
Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
```

 另外，还可以使用 Svcutil.exe 来处理文件系统中的 Web 服务描述语言 (WSDL) 和 XML 架构定义语言 (XSD) 文件。

```console
Svcutil.exe <list of WSDL and XSD files on file system>
```

 结果是一个代码文件，其中包含客户端应用程序可用于调用服务的 WCF 客户端代码。

 您还可以使用该工具生成配置文件。

```console
Svcutil.exe <file1 [,file2]>
```

 如果仅提供了一个文件名，则该文件名是输出文件的名称。 如果提供了两个文件名，则第一个文件是输入配置文件，其内容将与生成的配置合并，然后写出到第二个文件中。 有关配置的详细信息，请参阅 [配置服务的绑定](configuring-bindings-for-wcf-services.md)。

> [!IMPORTANT]
> 与任何未受保护的网络请求一样，未受保护的元数据请求也会带来一定的风险：如果您不能确定您正在与其进行通信的终结点身份属实，那么您检索的信息有可能是来自于恶意服务的元数据。

## <a name="add-service-reference-in-visual-studio"></a>Visual Studio 中的“添加服务引用”功能

 在运行服务的情况下，右键单击将包含 WCF 客户端代理的项目，然后选择 "**添加**  >  **服务引用**"。 在 " **添加服务引用" 对话框** 中，键入要调用的服务的 URL，然后单击 " **开始** " 按钮。 该对话框将显示在您指定的地址上提供的服务列表。 双击服务以查看可用的协定和操作，为生成的代码指定命名空间，然后单击 **"确定"** 按钮。

## <a name="example"></a>示例

 下面的代码示例演示为服务创建的一个服务协定。

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    // Other methods are not shown here.
}
```

```vb
' Define a service contract.
<ServiceContract(Namespace:="http://Microsoft.ServiceModel.Samples")> _
Public Interface ICalculator
    <OperationContract()>  _
    Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
    ' Other methods are not shown here.
End Interface
```

 在 Visual Studio 中的元数据实用工具工具和 **添加服务引用** 生成以下 WCF 客户端类。 该类从 <xref:System.ServiceModel.ClientBase%601> 泛型类继承，并实现 `ICalculator` 接口。 该工具还生成 `ICalculator` 接口（此处未演示）。

```csharp
public partial class CalculatorClient : System.ServiceModel.ClientBase<ICalculator>, ICalculator
{
    public CalculatorClient()
    {}

    public CalculatorClient(string endpointConfigurationName) :
            base(endpointConfigurationName)
    {}

    public CalculatorClient(string endpointConfigurationName, string remoteAddress) :
            base(endpointConfigurationName, remoteAddress)
    {}

    public CalculatorClient(string endpointConfigurationName,
        System.ServiceModel.EndpointAddress remoteAddress) :
            base(endpointConfigurationName, remoteAddress)
    {}

    public CalculatorClient(System.ServiceModel.Channels.Binding binding,
        System.ServiceModel.EndpointAddress remoteAddress) :
            base(binding, remoteAddress)
    {}

    public double Add(double n1, double n2)
    {
        return base.Channel.Add(n1, n2);
    }
}
```

```vb
Partial Public Class CalculatorClient
    Inherits System.ServiceModel.ClientBase(Of ICalculator)
    Implements ICalculator

    Public Sub New()
        MyBase.New
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String)
        MyBase.New(endpointConfigurationName)
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String, ByVal remoteAddress As String)
        MyBase.New(endpointConfigurationName, remoteAddress)
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String,
        ByVal remoteAddress As System.ServiceModel.EndpointAddress)
        MyBase.New(endpointConfigurationName, remoteAddress)
    End Sub

    Public Sub New(ByVal binding As System.ServiceModel.Channels.Binding,
        ByVal remoteAddress As System.ServiceModel.EndpointAddress)
        MyBase.New(binding, remoteAddress)
    End Sub

    Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
        Implements ICalculator.Add
        Return MyBase.Channel.Add(n1, n2)
    End Function
End Class
```

## <a name="using-the-wcf-client"></a>使用 WCF 客户端

 若要使用 WCF 客户端，请创建 WCF 客户端的实例，然后调用其方法，如下面的代码所示。

```csharp
// Create a client object with the given client endpoint configuration.
CalculatorClient calcClient = new CalculatorClient("CalculatorEndpoint"));
// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = calcClient.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
```

```vb
' Create a client object with the given client endpoint configuration.
Dim calcClient As CalculatorClient = _
New CalculatorClient("CalculatorEndpoint")

' Call the Add service operation.
Dim value1 As Double = 100.00D
Dim value2 As Double = 15.99D
Dim result As Double = calcClient.Add(value1, value2)
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)
```

## <a name="debugging-exceptions-thrown-by-a-client"></a>调试客户端引发的异常

WCF 客户端引发的许多异常由服务上的异常引起。 这些功能的示例包括：

- <xref:System.Net.Sockets.SocketException>: 现有连接被远程主机强行关闭。

- <xref:System.ServiceModel.CommunicationException>: 基础连接意外关闭。

- <xref:System.ServiceModel.CommunicationObjectAbortedException>: 套接字连接已中止。 这可能是由于处理消息时出错或远程主机超过接收超时或者潜在的网络资源问题导致的。

当发生这些类型的异常时，解决问题的最佳方式是在服务端启用跟踪并确定服务端发生了何种异常。 有关跟踪的详细信息，请参阅 [跟踪](./diagnostics/tracing/index.md) 和 [使用跟踪对应用程序进行故障排除](./diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)。

## <a name="see-also"></a>另请参阅

- [如何：创建客户端](how-to-create-a-wcf-client.md)
- [如何：使用双工协定访问服务](./feature-details/how-to-access-services-with-a-duplex-contract.md)
- [如何：以异步方式调用服务操作](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)
- [如何：使用单向和请求-答复协定访问服务](./feature-details/how-to-access-wcf-services-with-one-way-and-request-reply-contracts.md)
- [如何：访问 WSE 3.0 服务](./feature-details/how-to-access-a-wse-3-0-service-with-a-wcf-client.md)
- [了解生成的客户端代码](./feature-details/understanding-generated-client-code.md)
- [如何：使用 XmlSerializer 改善 WCF 客户端应用程序的启动时间](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)
- [指定客户端运行时行为](specifying-client-run-time-behavior.md)
- [配置客户端行为](configuring-client-behaviors.md)
