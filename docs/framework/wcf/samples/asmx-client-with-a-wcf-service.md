---
title: 带有 WCF 服务的 ASMX 客户端
ms.date: 03/30/2017
ms.assetid: 3ea381ee-ac7d-4d62-8c6c-12dc3650879f
ms.openlocfilehash: fd13d4907f1be09440387a36e14ecdc4926ba7e7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594772"
---
# <a name="asmx-client-with-a-wcf-service"></a>带有 WCF 服务的 ASMX 客户端

此示例演示如何使用 Windows Communication Foundation （WCF）创建服务，然后从非 WCF 客户端（如 .ASMX 客户端）访问该服务。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

此示例由客户端控制台程序 (.exe) 和 Internet 信息服务 (IIS) 所承载的服务库 (.dll) 组成。 该服务实现定义“请求-答复”通信模式的协定。 该协定由 `ICalculator` 接口定义，此接口公开数学运算（`Add`、`Subtract`、`Multiply` 和 `Divide`）。 ASMX 客户端向某个数学运算发出同步请求，服务使用结果进行回复。

该服务实现一个 `ICalculator` 协定，下面的代码对该协定进行了定义。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"), XmlSerializerFormat]
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

<xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Xml.Serialization.XmlSerializer> 将 CLR 类型映射到 XML 表示形式。 <xref:System.Runtime.Serialization.DataContractSerializer> 对某些 XML 表示形式的解释不同于 XmlSerializer。 当使用 XmlSerializer 时，非 WCF 代理生成器（如 Wsdl.exe）会生成一个更易于使用的接口。 <xref:System.ServiceModel.XmlSerializerFormatAttribute>应用于 `ICalculator` 接口，以确保使用 XMLSERIALIZER 将 CLR 类型映射到 XML。 服务实现计算并返回相应的结果。

服务公开单一终结点，以便与使用配置文件 (Web.config) 定义的服务进行通信。 终结点由地址、绑定和协定组成。 服务在 Internet 信息服务 (IIS) 主机提供的基地址公开该终结点。 `binding` 属性设置为 basicHttpBinding，它使用 SOAP 1.1（符合 WS-I BasicProfile 1.1）提供 HTTP 通信，如下面的示例配置所示。

```xml
<services>
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <!-- This endpoint is exposed at the base address provided by the host: http://localhost/servicemodelsamples/service.svc.  -->
    <endpoint address=""
              binding="basicHttpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
  </service>
</services>
```

使用由 Web 服务描述语言（WSDL）实用程序（Wsdl.exe）生成的类型化代理来与 WCF 服务进行通信。 该类型化代理包含在 generatedClient.cs 文件中。 WSDL 实用工具为指定的服务检索元数据并生成一个类型化代理，供客户端用来进行通信。 默认情况下，框架不公开任何元数据。 若要公开生成代理所需的元数据，必须添加 [\<serviceMetadata>](../../configure-apps/file-schema/wcf/servicemetadata.md) ，并将其 `httpGetEnabled` 属性设置为， `True` 如下面的配置所示。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
      <!-- Setting httpGetEnabled to True on the serviceMetadata
           behavior exposes the service's wsdl at <base address>?wsdl :
           http://localhost/servicemodelsamples/service.svc?wsdl -->
      <serviceMetadata httpGetEnabled="True"/>
      <serviceDebug includeExceptionDetailInFaults="False" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```

在客户端目录中通过命令提示符运行以下命令可以生成该类型化代理。

```console
wsdl /n:Microsoft.ServiceModel.Samples /o:generatedClient.cs /urlkey:CalculatorServiceAddress http://localhost/servicemodelsamples/service.svc?wsdl
```

通过使用生成的类型化代理，客户端可以通过配置相应的地址来访问给定的服务终结点。 客户端使用配置文件 (App.config) 指定要与其通信的终结点。

```xml
<appSettings>
  <add key="CalculatorServiceAddress"
       value="http://localhost/ServiceModelSamples/service.svc"/>
</appSettings>
```

客户端实现构造了类型化代理的一个实例，以开始与服务进行通信。

```csharp
// Create a client to the CalculatorService.
using (CalculatorService client = new CalculatorService())
{
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

}

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

> [!NOTE]
> 有关传递和返回复杂数据类型的详细信息，请参阅： [Windows 窗体客户端中的数据绑定](data-binding-in-a-windows-forms-client.md)、 [Windows Presentation Foundation 客户端中的数据](data-binding-in-a-wpf-client.md)绑定，以及[ASP.NET 客户端中的数据绑定](data-binding-in-an-aspnet-client.md)

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Interop\ASMX`
