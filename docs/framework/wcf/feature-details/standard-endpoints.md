---
title: 标准终结点
ms.date: 03/30/2017
ms.assetid: 3fcb4225-addc-44f2-935d-30e4943a8812
ms.openlocfilehash: a2f8d45990c5bc845aeee87ee82a2d043d6d1292
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96246441"
---
# <a name="standard-endpoints"></a>标准终结点

通过指定地址、绑定和协定来定义终结点。 终结点上可以设置的其他参数包括行为配置、标头和侦听 URI。  对于特定类型的终结点，这些值不会更改。 例如，元数据交换终结点始终使用 <xref:System.ServiceModel.Description.IMetadataExchange> 协定， 其他终结点（如 <xref:System.ServiceModel.Description.WebHttpEndpoint>）始终需要指定的终结点行为。 通过为常用终结点属性设置默认值可以提高终结点的可用性。 开发人员可以通过标准终结点定义具有默认值的终结点，或者定义一个或多个终结点属性不会更改的终结点。  这些终结点的优点是，可以使用此类终结点而无需指定静态性质的信息。 标准终结点可供基础结构终结点和应用程序终结点使用。  
  
## <a name="infrastructure-endpoints"></a>基础结构终结点  

 服务可能会公开这样的终结点：其中部分属性未由服务作者显式实现。 例如，元数据交换终结点公开 <xref:System.ServiceModel.Description.IMetadataExchange> 协定，但服务作者并不实现该接口，而是由 WCF 实现。 此类基础结构终结点的一个或多个终结点属性具有默认值，其中部分属性可能无法更改。 元数据交换终结点的 <xref:System.ServiceModel.Description.ServiceEndpoint.Contract%2A> 属性必须为 <xref:System.ServiceModel.Description.IMetadataExchange>，而其他属性（如绑定）可由开发人员提供。 通过将 <xref:System.ServiceModel.Description.ServiceEndpoint.IsSystemEndpoint%2A> 属性设置为 `true` 来标识基础结构终结点。  
  
## <a name="application-endpoints"></a>应用程序终结点  

 应用程序开发人员可以定义自己的标准终结点，为地址、绑定或协定指定默认值。 可以从 <xref:System.ServiceModel.Description.ServiceEndpoint> 派生类并设置相应的终结点属性来定义标准终结点。 可以为能够更改的属性提供默认值。 其他一些属性将具有无法更改的静态值。 下面的示例演示如何实现标准终结点。  
  
```csharp
public class CustomEndpoint : ServiceEndpoint
{
    public CustomEndpoint()
        : this(string.Empty)
    { }  

    public CustomEndpoint(string address)
        : this(address, ContractDescription.GetContract(typeof(ICalculator)))
    { }  

    // Create the custom endpoint with a fixed binding
    public CustomEndpoint(string address, ContractDescription contract)
        : base(contract)
    {
        this.Binding = new BasicHttpBinding();
        this.IsSystemEndpoint = false;
    }

    // Definition of the additional property of this endpoint
    public bool Property { get; set; }
}
```
  
 若要在配置文件中使用用户定义的自定义终结点，你必须从派生一个类 <xref:System.ServiceModel.Configuration.StandardEndpointElement> ，从派生一个类， <xref:System.ServiceModel.Configuration.StandardEndpointCollectionElement%602> 并在 app.config 或 machine.config 的 extensions 节中注册新的标准终结点。 <xref:System.ServiceModel.Configuration.StandardEndpointElement> 为标准终结点提供配置支持，如以下示例中所示。  
  
```csharp
public class CustomEndpointElement : StandardEndpointElement
{
    // Definition of the additional property for the standard endpoint element
    public bool Property
    {
        get { return (bool)base["property"]; }
        set { base["property"] = value; }
    }

    // The additional property needs to be added to the properties of the standard endpoint element
    protected override ConfigurationPropertyCollection Properties
    {
        get
        {
            ConfigurationPropertyCollection properties = base.Properties;
            properties.Add(new ConfigurationProperty("property", typeof(bool), false, ConfigurationPropertyOptions.None));
            return properties;
        }
    }

    // Return the type of this standard endpoint
    protected override Type EndpointType
    {
        get { return typeof(CustomEndpoint); }
    }

    // Create the custom service endpoint
    protected override ServiceEndpoint CreateServiceEndpoint(ContractDescription contract)
    {
        return new CustomEndpoint();
    }

    // Read the value given to the property in config and save it
    protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ServiceEndpointElement serviceEndpointElement)
    {
        CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;
        customEndpoint.Property = this.Property;
    }

    // Read the value given to the property in config and save it
    protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ChannelEndpointElement channelEndpointElement)
    {
        CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;
        customEndpoint.Property = this.Property;
    }

    // No validation in this sample
    protected override void OnInitializeAndValidate(ServiceEndpointElement serviceEndpointElement)
    {
    }

    // No validation in this sample
    protected override void OnInitializeAndValidate(ChannelEndpointElement channelEndpointElement)
    {
    }
}
```  
  
 为在 <xref:System.ServiceModel.Configuration.StandardEndpointCollectionElement%602> `standardEndpoints` 标准终结点的配置中 <> 部分下显示的集合提供支持类型。  下面的示例演示如何实现此类。  
  
```csharp
public class CustomEndpointCollectionElement : StandardEndpointCollectionElement<CustomEndpoint, CustomEndpointElement>
{
    // ...
}
```

下面的示例演示如何在 extensions 节中注册标准终结点。

```xml  
<extensions>  
      <standardEndpointExtensions>  
        <add  
          name="customStandardEndpoint"  
          type="CustomEndpointCollectionElement, Example.dll,  
                Version=1.0.0.0, Culture=neutral, PublicKeyToken=ffffffffffffffff"/>  
      </standardEndpointExtensions>
</extensions>  
```  
  
## <a name="configuring-a-standard-endpoint"></a>配置标准终结点  

 可以将标准终结点添加到代码或配置中。  若要将标准终结点添加到代码中，只需实例化相应的标准终结点类型，然后将标准终结点添加到服务主机，如下面的示例所示：  
  
```csharp  
serviceHost.AddServiceEndpoint(new CustomEndpoint());  
```  
  
 若要在配置中添加标准终结点，请将 <`endpoint`> 元素添加到 <`service`> 元素中，并将任何所需的配置设置添加到 <`standardEndpoints`> 元素中。 下面的示例演示如何添加 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>，它是随 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 一起提供的标准终结点之一。  
  
```xml  
<services>  
  <service>  
    <endpoint isSystemEndpoint="true" kind="udpDiscoveryEndpoint" />  
  </service>  
</services>  
<standardEndpoints>
  <udpDiscoveryEndpoint>  
     <standardEndpoint multicastAddress="soap.udp://239.255.255.250:3702" />
  </udpDiscoveryEndpoint>
</standardEndpoints>
```  
  
 使用 <> 元素中的 kind 特性指定标准终结点的类型 `endpoint` 。 终结点在 <> 元素中进行配置 `standardEndpoints` 。 在上例中，添加并配置了一个 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 终结点。 <`udpDiscoveryEndpoint`> 元素包含 `standardEndpoint` 设置的属性的 <> <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint.MulticastAddress%2A> <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 。  
  
## <a name="standard-endpoints-shipped-with-the-net-framework"></a>随 .NET Framework 一起提供的标准终结点  

 下表列出了随 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 一起提供的标准终结点。  
  
 `Mex Endpoint`  
 用于公开服务元数据的标准终结点。  
  
 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>  
 由服务用于发送公告消息的标准终结点。  
  
 <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>  
 由服务用于发送发现消息的标准终结点。  
  
 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>  
 通过 UDP 多播绑定为发现操作预配的标准终结点。  
  
 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>  
 由服务用于通过 UDP 绑定发送公告消息的标准终结点。  
  
 <xref:System.ServiceModel.Discovery.DynamicEndpoint>  
 使用 WS-Discovery 在运行时动态查找终结点地址的标准终结点。  
  
 <xref:System.ServiceModel.Description.ServiceMetadataEndpoint>  
 用于元数据交换的标准终结点。  
  
 <xref:System.ServiceModel.Description.WebHttpEndpoint>  
 带有自动添加 <xref:System.ServiceModel.WebHttpBinding> 行为的 <xref:System.ServiceModel.Description.WebHttpBehavior> 绑定的标准终结点。  
  
 <xref:System.ServiceModel.Description.WebScriptEndpoint>  
 带有自动添加 <xref:System.ServiceModel.WebHttpBinding> 行为的 <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> 绑定的标准终结点。  
  
 <xref:System.ServiceModel.Description.WebServiceEndpoint>  
 带有 <xref:System.ServiceModel.WebHttpBinding> 绑定的标准终结点。  
  
 <xref:System.ServiceModel.Activities.WorkflowControlEndpoint>  
 可用于对工作流实例调用控制操作的标准终结点。  
  
 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint>  
 支持工作流创建和书签恢复的标准终结点。
