---
title: 标准终结点的用法
ms.date: 03/30/2017
ms.assetid: ecd6a62f-9619-4778-a497-6f888087a9ea
ms.openlocfilehash: 2b210bfe683669aeebf54a1701f07d492e6abdb4
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715348"
---
# <a name="usage-of-standard-endpoints"></a>标准终结点的用法

此示例演示如何在服务配置文件中使用标准终结点。 通过标准终结点，用户可以使用单个属性来描述地址、绑定和协定组合以及与其关联的附加属性，从而简化终结点定义。 此示例演示如何定义和实现自定义标准终结点，以及如何在终结点中定义特定属性。

## <a name="sample-details"></a>示例详细信息

可以通过指定地址、绑定和协定这三个参数来指定服务终结点。 可以提供的其他参数包括行为配置、标头、侦听 URI 等。 在某些情况下，任何或所有地址、绑定和协定具有无法更改的值。 因此，可以使用标准终结点。 这类终结点包括元数据交换终结点和发现终结点。 标准终结点还允许配置服务终结点，而不必提供固定性质的信息或创建其自己的标准终结点，从而提高了可用性，例如通过提供一组合理的默认值，从而降低配置文件的详细级别，来提高可用性。

此示例由两个项目组成：定义两个标准终结点的服务以及与该服务通信的客户端。 下面的示例演示如何为配置文件中的服务定义标准终结点。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <extensions>
      <endpointExtensions>
        <add name="customEndpoint" type="Microsoft.Samples.StandardEndpoints.CustomEndpointCollectionElement, service" />
      </endpointExtensions>
    </extensions>
    <services>
      <service name="Microsoft.Samples.StandardEndpoints.CalculatorService">
        <endpoint address="http://localhost:8000/Samples/Service.svc/customEndpoint" contract="Microsoft.Samples.StandardEndpoints.ICalculator" kind="customEndpoint" />
        <endpoint kind="mexEndpoint" />
      </service>
    </services>
    <behaviors>
      <serviceBehaviors>
        <behavior>
          <serviceMetadata httpGetEnabled="True"/>
        </behavior>
      </serviceBehaviors>
    </behaviors>
    <standardEndpoints>
      <customEndpoint>
        <standardEndpoint property="True" />
      </customEndpoint>
    </standardEndpoints>
  </system.serviceModel>
</configuration>
```

为服务定义的第一个终结点的类型为 `customEndpoint`，其定义在 `<standardEndpoints>` 节中，其中为属性 `property` 提供了值 `true`。 这是使用新属性自定义终结点的情况。 第二个终结点对应于元数据终结点，其中地址、绑定和协定的值是固定的。

若要定义标准终结点元素，必须创建从 `StandardEndpointElement` 派生的类。 在此示例中，已定义了 `CustomEndpointElement` 类，如下面的示例所示。

```csharp
public class CustomEndpointElement : StandardEndpointElement
{
    public bool Property
    {
        get { return (bool)base["property"]; }
        set { base["property"] = value; }
    }

    protected override ConfigurationPropertyCollection Properties
    {
        get
        {
            ConfigurationPropertyCollection properties = base.Properties;
            properties.Add(new ConfigurationProperty("property", typeof(bool), false, ConfigurationPropertyOptions.None));
            return properties;
        }
    }

    protected override Type EndpointType
    {
        get { return typeof(CustomEndpoint); }
    }

    protected override ServiceEndpoint CreateServiceEndpoint(ContractDescription contract)
    {
        return new CustomEndpoint();
    }

    protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ServiceEndpointElement serviceEndpointElement)
    {
        CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;
        customEndpoint.Property = this.Property;
    }

    protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ChannelEndpointElement channelEndpointElement)
    {
        CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;
        customEndpoint.Property = this.Property;
    }

    protected override void OnInitializeAndValidate(ServiceEndpointElement serviceEndpointElement)
    {
    }

    protected override void OnInitializeAndValidate(ChannelEndpointElement channelEndpointElement)
    {
    }
}
```

在 `CreateServiceEndpoint` 函数中，创建了一个 `CustomEndpoint` 对象。 下面的示例演示了其定义：

```csharp
public class CustomEndpoint : ServiceEndpoint
{
    public CustomEndpoint()
        : this(string.Empty)
    {
    }

    public CustomEndpoint(string address)
        : this(address, ContractDescription.GetContract(typeof(ICalculator)))
    {
    }

    public CustomEndpoint(string address, ContractDescription contract)
        : base(contract)
    {
        this.Binding = new BasicHttpBinding();
        this.IsSystemEndpoint = false;
    }

    public bool Property
    {
        get;
        set;
    }
}
```

 若要在服务与客户端之间执行通信，需要在客户端中创建对服务的服务引用。 当生成和执行示例时，服务会执行，且客户端与之进行通信。 请注意，每次对服务进行更改时，都应更新服务引用。

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2012 打开 StandardEndpoints 文件。

2. 使多个项目可以启动。

    1. 在**解决方案资源管理器**中，右键单击 "标准终结点" 解决方案，然后选择 "**属性**"。

    2. 在 "**通用属性**" 中，选择 "**启动项目**"，然后单击 "**多个启动项目**"。

    3. 将服务项目移动到列表的开头，将 "**操作**" 设置为 "**启动**"。

    4. 将客户端项目移动到服务项目之后，同时将**操作**设置为 "**启动**"。

         这指定客户端项目在服务项目之后执行。

3. 若要运行解决方案，请按 F5。

> [!NOTE]
> 如果这些步骤不起作用，请使用以下步骤确保已正确设置你的环境：
>
> 1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。
> 2. 若要生成解决方案，请按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。
> 3. 若要在一台或多台计算机配置中运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\StandardEndpoints`
