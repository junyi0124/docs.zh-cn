---
title: 使用行为配置和扩展运行时
description: 了解如何在 WCF 应用中实现行为接口，并以编程方式或在配置文件中将其添加到服务说明或终结点。
ms.date: 03/30/2017
helpviewer_keywords:
- attaching extensions using behaviors [WCF]
ms.assetid: 149b99b6-6eb6-4f45-be22-c967279677d9
ms.openlocfilehash: dc2f7a63c92664d5ba20e8ce0c70129907299ce9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257875"
---
# <a name="configuring-and-extending-the-runtime-with-behaviors"></a>使用行为配置和扩展运行时

利用行为，你可以修改默认行为，并添加用于在 Windows Communication Foundation (WCF) 客户端和服务应用程序中检查和验证服务配置或修改运行时行为的自定义扩展。 本主题说明行为接口、如何实现这些接口以及如何以编程方式将它们添加到服务说明（在服务应用程序中）或终结点（在客户端应用程序中）或配置文件中。 有关使用系统提供的行为的详细信息，请参阅 [指定服务 Run-Time 行为](../specifying-service-run-time-behavior.md) 和 [指定客户端 Run-Time 行为](../specifying-client-run-time-behavior.md)。  
  
## <a name="behaviors"></a>行为  

 行为类型将分别添加到服务或服务终结点说明对象 (在服务或客户端上，Windows Communication Foundation (WCF) 来创建执行 WCF 服务或 WCF 客户端的运行时，这些对象在使用这些对象前) 。 在运行时构造过程中调用这些行为时，这些行为可以访问运行时属性和方法以修改由协定、绑定和地址构造的运行时。  
  
### <a name="behavior-methods"></a>行为方法  

 所有行为都具有一个 `AddBindingParameters` 方法、一个 `ApplyDispatchBehavior` 方法、一个 `Validate` 方法和一个 `ApplyClientBehavior` 方法，但有一个例外：因为 <xref:System.ServiceModel.Description.IServiceBehavior> 无法在客户端中执行，因此该行为不实现 `ApplyClientBehavior`。  
  
- 使用 `AddBindingParameters` 方法可修改自定义对象或将自定义对象添加到集合，在构造运行时时，自定义绑定可以访问该集合以使这些对象。 例如，可能会指定影响通道生成方式的保护要求，但通道开发人员可能并不知道这些保护要求。  
  
- 使用 `Validate` 方法可检查说明树和相应的运行时对象以确保符合某一条件集。  
  
- 使用 `ApplyDispatchBehavior` 和 `ApplyClientBehavior` 方法可检查说明树并在服务或客户端上修改特定范围的运行时。 也可以插入扩展对象。  
  
    > [!NOTE]
    > 尽管在这些方法中提供了说明树，但它仅限用于检查。 如果修改了说明树，则行为将是不确定的。  
  
 可以修改的属性和可以实现的自定义接口可通过服务和客户端运行时类访问。 服务类型为 <xref:System.ServiceModel.Dispatcher.DispatchRuntime> 和 <xref:System.ServiceModel.Dispatcher.DispatchOperation> 类。 客户端类型为 <xref:System.ServiceModel.Dispatcher.ClientRuntime> 和 <xref:System.ServiceModel.Dispatcher.ClientOperation> 类。 <xref:System.ServiceModel.Dispatcher.ClientRuntime> 和 <xref:System.ServiceModel.Dispatcher.DispatchRuntime> 类是分别访问客户端范围和服务范围运行时属性和扩展集合的扩展入口点。 同样，<xref:System.ServiceModel.Dispatcher.ClientOperation> 和 <xref:System.ServiceModel.Dispatcher.DispatchOperation> 类分别公开客户端操作和服务操作运行时属性和扩展集合。 但您可以从操作运行时对象访问更广范围的运行时对象，如果需要，反之亦然。  
  
> [!NOTE]
> 有关可用于修改客户端执行行为的运行时属性和扩展类型的讨论，请参阅 [扩展客户端](extending-clients.md)。 有关可用于修改服务调度程序执行行为的运行时属性和扩展类型的讨论，请参阅 [扩展调度](extending-dispatchers.md)程序。  
  
 大多数 WCF 用户不会直接与运行时交互;相反，它们在配置文件中使用类或行为的核心编程模型构造，如终结点、协定、绑定、地址和行为特性。 这些构造构成了 *说明树*，它是构造运行时以支持说明树描述的服务或客户端的完整规范。  
  
 WCF 中有四种行为：  
  
- 服务行为（<xref:System.ServiceModel.Description.IServiceBehavior> 类型）启用整个服务运行时（包括 <xref:System.ServiceModel.ServiceHostBase>）的自定义。  
  
- 终结点行为（<xref:System.ServiceModel.Description.IEndpointBehavior> 类型）启用服务终结点及其关联 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> 对象的自定义。  
  
- 协定行为（<xref:System.ServiceModel.Description.IContractBehavior> 类型）分别启用客户端和服务应用程序中 <xref:System.ServiceModel.Dispatcher.ClientRuntime> 和 <xref:System.ServiceModel.Dispatcher.DispatchRuntime> 类的自定义。  
  
- 操作行为（<xref:System.ServiceModel.Description.IOperationBehavior> 类型）也启用客户端和服务中 <xref:System.ServiceModel.Dispatcher.ClientOperation> 和 <xref:System.ServiceModel.Dispatcher.DispatchOperation> 类的自定义。  
  
 您可以通过实现自定义属性、使用应用程序配置文件或直接将行为添加到相应说明对象上的行为集合，将这些行为添加到各个说明对象。 但在对 <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.ServiceHost> 调用 <xref:System.ServiceModel.ChannelFactory%601> 之前，必须将这些行为添加到服务说明或服务终结点说明对象。  
  
### <a name="behavior-scopes"></a>行为范围  

 有四种行为类型，每种类型各对应于特定范围的运行时访问。  
  
#### <a name="service-behaviors"></a>服务行为  

 服务行为实现 <xref:System.ServiceModel.Description.IServiceBehavior>，它是赖以修改整个服务运行时的主要机制。 向服务中添加服务行为有三种方式。  
  
1. 在服务类上使用属性。  在构造 <xref:System.ServiceModel.ServiceHost> 时，<xref:System.ServiceModel.ServiceHost> 实现使用反射来发现服务类型上的属性集。 如果这些属性中的任何属性是 <xref:System.ServiceModel.Description.IServiceBehavior> 的实现，则会将其添加到 <xref:System.ServiceModel.Description.ServiceDescription> 上的行为集合中。 这允许这些行为参与服务运行时的构造。  
  
2. 以编程方式将行为添加到 <xref:System.ServiceModel.Description.ServiceDescription> 上的行为集合中。 这可以通过以下几行代码实现：  
  
    ```csharp
    ServiceHost host = new ServiceHost(/* Parameters */);  
    host.Description.Behaviors.Add(/* Service Behavior */);  
    ```  
  
3. 实现用于扩展配置的自定义 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement>。 这将允许在应用程序配置文件中使用服务行为。  
  
 WCF 中服务行为的示例包括 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性、 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior> 和 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 行为。  
  
#### <a name="contract-behaviors"></a>协定行为  

 协定行为实现 <xref:System.ServiceModel.Description.IContractBehavior> 接口，它用于在协定范围内扩展客户端和服务运行时。  
  
 向协定中添加协定行为有两种方式。  第一种方式是创建要在协定接口上使用的自定义属性。 当协定接口传递到 <xref:System.ServiceModel.ServiceHost> 或时 <xref:System.ServiceModel.ChannelFactory%601> ，WCF 将检查该接口上的特性。 如果任何属性是 <xref:System.ServiceModel.Description.IContractBehavior> 的实现，则会将其添加到为该接口创建的 <xref:System.ServiceModel.Description.ContractDescription?displayProperty=nameWithType> 上的行为集合中。  
  
 也可以在自定义协定行为属性上实现 <xref:System.ServiceModel.Description.IContractBehaviorAttribute?displayProperty=nameWithType>。 在这种情况下，当应用于以下对象时，行为如下所述：  
  
 • 协定接口。 在这种情况下，该行为将应用到任何终结点中该类型的所有协定，WCF 将忽略该属性的值 <xref:System.ServiceModel.Description.IContractBehaviorAttribute.TargetContract%2A?displayProperty=nameWithType> 。  
  
 • 服务类。 在此情况下，该行为只应用到其协定是 <xref:System.ServiceModel.Description.IContractBehaviorAttribute.TargetContract%2A> 属性的值的终结点。  
  
 • 回调类。 在这种情况下，该行为将应用到双工客户端的终结点，WCF 将忽略属性的值 <xref:System.ServiceModel.Description.IContractBehaviorAttribute.TargetContract%2A> 。  
  
 第二种方式是将该行为添加到 <xref:System.ServiceModel.Description.ContractDescription> 上的行为集合中。  
  
 WCF 中协定行为的示例包括 <xref:System.ServiceModel.DeliveryRequirementsAttribute?displayProperty=nameWithType> 特性。 有关更多信息和示例，请参见参考主题。  
  
#### <a name="endpoint-behaviors"></a>终结点行为  

 终结点行为实现 <xref:System.ServiceModel.Description.IEndpointBehavior>，它是赖以修改特定终结点的整个服务或客户端运行时的主要机制。  
  
 向服务中添加终结点行为有两种方式。  
  
1. 将行为添加到 <xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> 属性。  
  
2. 实现用于扩展配置的自定义 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement>。  
  
 有关更多信息和示例，请参见参考主题。  
  
#### <a name="operation-behaviors"></a>操作行为  

 操作行为实现 <xref:System.ServiceModel.Description.IOperationBehavior> 接口，用于扩展每个操作的客户端和服务运行时。  
  
 向操作中添加操作行为有两种方式。 第一种方式是创建要在对操作建模的方法上使用的自定义属性。 将操作添加到 <xref:System.ServiceModel.ServiceHost> 或时 <xref:System.ServiceModel.ChannelFactory> ，WCF 会将所有  <xref:System.ServiceModel.Description.IOperationBehavior> 特性添加到 <xref:System.ServiceModel.Description.OperationDescription> 为该操作创建的上的行为集合中。  
  
 第二种方式是直接将该行为添加到所构造的 <xref:System.ServiceModel.Description.OperationDescription> 上的行为集合中。  
  
 WCF 中的操作行为的示例包括 <xref:System.ServiceModel.OperationBehaviorAttribute> 和 <xref:System.ServiceModel.TransactionFlowAttribute> 。  
  
 有关更多信息和示例，请参见参考主题。  
  
### <a name="using-configuration-to-create-behaviors"></a>使用配置创建行为  

 服务和终结点以及协定行为可以设计成使用代码或属性进行指定；只有服务和终结点行为才可以使用应用程序或 Web 配置文件进行配置。 使用属性来公开行为可以让开发人员在编辑时指定行为，而该行为在运行时是无法添加、删除或修改的。 这种方式通常适合于服务的正确操作始终需要的那些行为（例如 <xref:System.ServiceModel.ServiceBehaviorAttribute?displayProperty=nameWithType> 属性的事务相关参数）。 使用配置来公开行为可以让开发人员将这些行为的指定和配置留给部署服务的那些人去完成。 这种方式适合于作为可选组件或其他特定于部署的配置的行为，比如是否为服务或服务的特定授权配置公开元数据。  
  
> [!NOTE]
> 也可以使用支持配置的行为，并通过将这些行为插入到 machine.config 配置文件并锁定这些项来强制执行公司的应用程序策略。 有关说明和示例，请参阅 [如何：锁定企业中的终结点](how-to-lock-down-endpoints-in-the-enterprise.md)。  
  
 若要使用配置来公开行为，开发人员必须创建 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement> 的派生类，然后向配置注册该扩展。  
  
 下面的代码示例演示 <xref:System.ServiceModel.Description.IEndpointBehavior> 如何实现 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement>：  
  
```csharp
// BehaviorExtensionElement members  
public override Type BehaviorType  
{  
  get { return typeof(EndpointBehaviorMessageInspector); }  
}  
  
protected override object CreateBehavior()  
{  
  return new EndpointBehaviorMessageInspector();  
}  
```  
  
 若要让配置系统加载自定义 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement>，必须将该类注册为扩展。 下面的代码示例演示前一个终结点行为的配置文件：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service
        name="Microsoft.WCF.Documentation.SampleService"  
        behaviorConfiguration="metadataSupport"  
      >  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8080/ServiceMetadata" />  
          </baseAddresses>  
        </host>  
        <endpoint  
          address="/SampleService"  
          binding="wsHttpBinding"  
          behaviorConfiguration="withMessageInspector"
          contract="Microsoft.WCF.Documentation.ISampleService"  
        />  
        <endpoint  
           address="mex"  
           binding="mexHttpBinding"  
           contract="IMetadataExchange"  
        />  
      </service>  
    </services>  
    <behaviors>  
      <serviceBehaviors>  
      <behavior name="metadataSupport">  
        <serviceMetadata httpGetEnabled="true" httpGetUrl=""/>  
      </behavior>  
      </serviceBehaviors>  
      <endpointBehaviors>  
        <behavior name="withMessageInspector">  
          <endpointMessageInspector />  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
    <extensions>  
      <behaviorExtensions>  
        <add
          name="endpointMessageInspector"  
          type="Microsoft.WCF.Documentation.EndpointBehaviorMessageInspector, HostApplication, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null"  
        />  
      </behaviorExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
 其中 `Microsoft.WCF.Documentation.EndpointBehaviorMessageInspector` 是行为扩展类型， `HostApplication` 是已编译该类的程序集的名称。  
  
### <a name="evaluation-order"></a>计算顺序  

 <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> 和 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> 负责从编程模型和说明生成运行时。 如前面所述，行为可在服务、终结点、协定和操作中参与该生成过程。  
  
 <xref:System.ServiceModel.ServiceHost> 按以下顺序应用行为：  
  
1. 服务  
  
2. 协定  
  
3. 终结点  
  
4. 操作  
  
 在任何行为集合内，不保证任何顺序。  
  
 <xref:System.ServiceModel.ChannelFactory%601> 按以下顺序应用行为：  
  
1. 协定  
  
2. 终结点  
  
3. 操作  
  
 在任何行为集合内，同样不保证任何顺序。  
  
### <a name="adding-behaviors-programmatically"></a>以编程方式添加行为  

 服务应用程序中 <xref:System.ServiceModel.Description.ServiceDescription?displayProperty=nameWithType> 的属性不能在 <xref:System.ServiceModel.Channels.CommunicationObject.OnOpening%2A?displayProperty=nameWithType> 上的 <xref:System.ServiceModel.ServiceHostBase?displayProperty=nameWithType> 方法之后进行修改。 有些成员（如 <xref:System.ServiceModel.ServiceHostBase.Credentials%2A?displayProperty=nameWithType> 属性以及 `AddServiceEndpoint` 和 <xref:System.ServiceModel.ServiceHostBase> 上的 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> 方法）在此时点后修改时会引发异常。 允许修改其他成员，但结果不可确定。  
  
 同样，在调用 <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType> 上的 <xref:System.ServiceModel.Channels.CommunicationObject.OnOpening%2A> 之后不能在客户端上修改 <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType> 值。 <xref:System.ServiceModel.ChannelFactory.Credentials%2A?displayProperty=nameWithType> 属性在此时点后修改时会引发异常，但其他客户端说明值可以正常修改而不会出现错误。 但结果不可确定。  
  
 无论是对于服务还是客户端，建议您在调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A?displayProperty=nameWithType> 之前修改说明。  
  
### <a name="inheritance-rules-for-behavior-attributes"></a>行为属性的继承规则  

 所有四种类型的行为都可以使用属性进行填充 – 服务行为和协定行为。 由于属性是在托管对象和成员上定义的，而托管对象和成员支持继承，因此有必要定义行为属性在继承上下文中的工作方式。  
  
 总的来说，规则是这样的：对于特定范围（例如服务、协定或操作），该范围的继承层次结构中的所有行为属性均适用。 如果两个行为属性具有相同的类型，则使用派生程度最高的类型。  
  
#### <a name="service-behaviors"></a>服务行为  

 对于给定的服务类，该类及该类的父类上的所有服务行为属性均适用。 如果在继承层次结构中的多个位置应用同种属性类型，则使用派生程度最高的类型。  
  
```csharp  
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Multiple)]  
[AspNetCompatibilityRequirementsAttribute(  
    AspNetCompatibilityRequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]  
public class A { /* … */ }  
  
[ServiceBehavior(InstanceContextMode = InstanceContextMode.Single)]  
public class B : A { /* … */}  
```  
  
 例如在上例中，服务 B 结束时，其 <xref:System.ServiceModel.InstanceContextMode> 为 <xref:System.ServiceModel.InstanceContextMode.Single>、<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode> 模式为 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> 且 <xref:System.ServiceModel.ConcurrencyMode> 为 <xref:System.ServiceModel.ConcurrencyMode.Single>。 <xref:System.ServiceModel.ConcurrencyMode> 为 <xref:System.ServiceModel.ConcurrencyMode.Single>，这是因为服务 B 上的 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性比服务 A 上的该属性“派生程度更高”。  
  
#### <a name="contract-behaviors"></a>协定行为  

 对于给定的协定，该接口及该接口的父接口上的所有协定行为属性均适用。 如果在继承层次结构中的多个位置应用同种属性类型，则使用派生程度最高的类型。  
  
#### <a name="operation-behaviors"></a>操作行为  

 如果给定操作不重写现有抽象或虚拟操作，则继承规则不适用。  
  
 如果操作重写现有操作，则该操作及该操作的父操作上的所有操作行为属性均适用。  如果在继承层次结构中的多个位置应用同种属性类型，则使用派生程度最高的类型。
