---
title: 服务版本控制
ms.date: 03/30/2017
ms.assetid: 37575ead-d820-4a67-8059-da11a2ab48e2
ms.openlocfilehash: fae0a5eca5737c3d7885cbe6c678678adabbea01
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245340"
---
# <a name="service-versioning"></a>服务版本控制

服务（及其公开的终结点）在初始部署之后，可能出于多种原因（例如，更改业务需求、信息技术需求，或者为了解决其他问题）而需要更改，并且在其生存期期间可能需要更改多次。 每次更改都会引入服务的一个新版本。 本主题说明如何在 Windows Communication Foundation (WCF) 中考虑版本控制。  
  
## <a name="four-categories-of-service-changes"></a>服务更改的四个类别  

 可能需要的服务更改可分成四类：  
  
- 协定更改：例如，可能添加某个操作，或者可能添加或更改消息中的某个数据元素。  
  
- 地址更改：例如，服务移至另一个位置，终结点在此位置获得新地址。  
  
- 绑定更改：例如，安全机制更改或其设置更改。  
  
- 实现更改：例如，当内部方法实现更改时。  
  
 这些更改中有一些称为“中断性”，而其他则为“非中断性”。 如果在早期版本中已成功处理的所有消息都已在新版本中成功处理 *，则更改* 不会中断。 不符合该条件的任何更改都是一项 *重大* 更改。  
  
## <a name="service-orientation-and-versioning"></a>面向服务和版本管理  

 面向服务的原则之一是服务和客户端自主（即独立）。 这其中的一层含义是，服务开发人员不能假设他们能控制，甚至是了解所有服务客户端。 这消除了在服务更改版本时重建和重新部署所有客户端的可能。 本主题假设服务遵循这一原则，并因此必须独立于其客户端进行更改或“版本变更”。  
  
 如果出现意外的并且无法避免的中断性更改，应用程序可选择忽略此原则，并要求以新版本的服务重建和重新部署客户端。  
  
## <a name="contract-versioning"></a>协定版本管理  

 客户端使用的协定不需要与服务所使用的协定相同；它们只需要兼容即可。  
  
 对于服务协定，兼容性意味着可以添加服务所公开的新操作，但是不能移除或从语义上更改现有操作。  
  
 对于数据协定，兼容性意味着可以添加新的架构类型定义，但是不能以中断性方式更改架构类型定义。 中断性更改可包括移除数据成员或不兼容地更改其数据类型。 这一特点允许服务在某种程度上更改其协定的版本而又不中断客户端。 接下来的两部分介绍了可对 WCF 数据和服务协定进行的不间断和重大更改。  
  
## <a name="data-contract-versioning"></a>数据协定版本管理  

 本节讨论使用 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.DataContractAttribute> 类时的数据版本管理。  
  
### <a name="strict-versioning"></a>严格版本管理  

 很多情况下，当更改版本是一个问题时，服务开发人员无法控制客户端，因此无法假设它们将对消息 XML 或架构中的更改作何反应。 在这些情况下，出于两个原因，必须保证将针对旧架构验证新消息：  
  
- 开发旧客户端时的假设是架构将不会更改。 这些旧客户端可能无法处理设计时并未考虑要处理的消息。  
  
- 旧客户端可能就在尝试处理消息之前针对旧架构执行实际架构验证。  
  
 此类情况下的建议方法是将现有数据协定视为不可变，并用唯一的 XML 限定名创建新协定。 服务开发人员然后可向现有服务协定添加新方法或者以使用新数据协定的方法创建新服务协定。  
  
 通常的情况是，服务开发人员需要编写一些业务逻辑，这些逻辑应在数据协定的所有版本内运行，此外还需要针对该数据协定的每个版本编写特定于版本的业务代码。 本主题最后的附录说明了如何使用接口来满足这种需要。  
  
### <a name="lax-versioning"></a>宽松版本管理  

 在很多其他情况下，服务开发人员可假设向数据协定添加新的可选成员不会中断现有客户端。 这要求服务开发人员调查现有客户端是否并不执行架构验证，以及它们是否忽略未知数据成员。 在这些情况下，就可以利用以非中断的方式添加新成员的数据协定功能。 如果数据协定的版本管理功能已经用于服务的第一个版本，服务开发人员就可以肯定地作此假设。  
  
 WCF、ASP.NET Web 服务和许多其他 Web 服务堆栈支持 *宽松版本控制*：也就是说，它们不会为收到的数据中的新未知数据成员引发异常。  
  
 很容易误以为添加新成员将不会中断现有客户端。 如果不确定所有客户端是否都能处理宽松版本管理，则建议使用严格版本管理准则，并将数据协定视为不可变。  
  
 有关数据协定的宽松和严格版本控制的详细指南，请参阅 [最佳做法：数据协定版本控制](best-practices-data-contract-versioning.md)。  
  
### <a name="distinguishing-between-data-contract-and-net-types"></a>区分数据协定和 .NET 类型  

 通过将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于类，.NET 类或结构可映射为数据协定。 .NET 类型与其数据协定映射二者完全不同。 同一个数据协定映射可能有多个 .NET 类型。 此区别尤其有益于允许在保留映射的数据协定的同时更改 .NET 类型，从而甚至在是严格意义上保持与现有客户端的兼容性。 为了保持 .NET 类型与数据协定之间的这一区别，始终应该执行两项操作：  
  
- 指定一个 <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> 和 <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A>。 应总是指定数据协定的名称和命名空间，以防止在协定中暴露 .NET 类型的名称和命名空间。 这样，如果以后决定更改 .NET 命名空间或类型名称，数据协定将保持不变。  
  
- 指定 <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A>。 应总是指定数据成员的名称，以防止在协定中暴露 .NET 成员名称。 这样，如果以后决定更改成员的 .NET 名称，数据协定将保持不变。  
  
### <a name="changing-or-removing-members"></a>更改或移除成员  

 即使是在允许宽松版本管理的情况下，更改成员的名称或数据类型或者移除数据成员仍然是中断性更改。 如果必须更改或移除，请创建新的数据协定。  
  
 如果服务兼容性高度重要，可以考虑忽略代码中未使用的数据成员并将其保留原位。 如果要将一个数据成员拆分成多个成员，则可以考虑将现有成员保留在原位，作为可为下级客户端（未升级到最新版本的客户端）执行所需的拆分和重新聚合的属性。  
  
 同样，对数据协定的名称或命名空间进行更改也是中断性更改。  
  
### <a name="round-trips-of-unknown-data"></a>未知数据的往返  

 某些情况下，需要“往返”传递来自于新版本中添加的成员的未知数据。 例如，“versionNew”服务将数据与某些新添加的成员一起发送到“versionOld”客户端。 客户端在处理该消息时将忽略新添加的成员，但会将相同的数据（包括新添加的成员）重新发送回 versionNew 服务。 此现象的典型情况是数据更新，更新过程从服务检索数据，更改数据，然后将其返回。  
  
 若要对某一特定类型启用往返功能，该类型必须实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口。 此接口包含一个属性 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A>，它将返回 <xref:System.Runtime.Serialization.ExtensionDataObject> 类型。 该属性用于存储未来版本的数据协定中对于当前版本未知的任何数据。 客户端无法处理这些数据，但在实例序列化的时候，<xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> 属性的内容将与数据协定成员的其余数据一起写入。  
  
 建议所有类型都实现此接口以接纳新的未知未来成员。  
  
### <a name="data-contract-libraries"></a>数据协定库  

 可以存在数据协定库，协定在这里发布到中心储存库，然后服务和类型实现者实现和公开来自于该储存库中的数据协定。 在此情况下，在将数据协定发布到储存库时，您无法控制谁将创建实现此协定的类型。 因此，在协定发布之后，就不能进行修改，这实际上是使协定不可变。  
  
### <a name="when-using-the-xmlserializer"></a>使用 XmlSerializer 的场合  

 在使用 <xref:System.Xml.Serialization.XmlSerializer> 类时，同样的版本管理原则依然适用。 在需要严格版本管理时，请将数据协定视为不可变，并为新版本创建具有唯一限定名的新数据协定。 如果确定可以使用宽松版本管理，则可在新版本中添加新的可序列化成员，但不能更改或移除现有成员。  
  
> [!NOTE]
> <xref:System.Xml.Serialization.XmlSerializer> 使用 <xref:System.Xml.Serialization.XmlAnyElementAttribute> 和 <xref:System.Xml.Serialization.XmlAnyAttributeAttribute> 属性来支持未知数据的往返。  
  
## <a name="message-contract-versioning"></a>消息协定版本管理  

 消息协定版本管理的准则非常类似于数据协定的版本管理。 如果需要严格版本管理，则不应更改消息主体，而是应该创建具有唯一限定名的新消息协定。 如果确定可以使用宽松版本管理，则可添加新的消息主体部件，但不能更改或移除现有部件。 此准则同时适用于裸消息协定和包装消息协定。  
  
 总是可以添加消息头，即使是使用了严格版本管理。 MustUnderstand 标志可能影响版本管理。 一般而言，WCF 中标头的版本控制模型如 SOAP 规范中所述。  
  
## <a name="service-contract-versioning"></a>服务协定版本管理  

 与数据协定版本管理相似，服务协定版本管理也涉及添加、更改和移除操作。  
  
### <a name="specifying-name-namespace-and-action"></a>指定名称、命名空间和动作  

 默认情况下，服务协定的名称就是接口的名称。 其默认命名空间为 " http://tempuri.org "，每个操作的操作为 " http://tempuri.org/contractname/methodname "。 建议为服务协定显式指定一个名称和命名空间，并为每个操作显式指定一个操作，以避免使用 " http://tempuri.org " 并防止接口和方法名称在服务的协定中公开。  
  
### <a name="adding-parameters-and-operations"></a>添加参数和操作  

 添加服务公开的服务操作是非中断性更改，因为现有客户端不需要考虑这些新操作。  
  
> [!NOTE]
> 向双工回调协定添加操作是中断性更改。  
  
### <a name="changing-operation-parameter-or-return-types"></a>更改操作参数或返回类型  

 更改参数或返回类型通常是中断性更改，除非新旧类型实现了相同的数据协定。 若要进行这样的更改，请向服务协定添加新操作，或者定义新的服务协定。  
  
### <a name="removing-operations"></a>移除操作  

 移除操作也是中断性更改。 若要进行这样的更改，请定义一个新的服务协定，并在新的终结点上公开该协定。  
  
### <a name="fault-contracts"></a>错误协定  

 <xref:System.ServiceModel.FaultContractAttribute> 属性使得服务协定开发人员能够指定有关可从协定的操作返回的错误的消息。  
  
 服务的协定中所描述的错误列表并非详尽无遗。 操作可能随时返回其协定中未描述的错误。 因此，更改协定中描述的错误集合不会被视为中断性更改。 例如，使用 <xref:System.ServiceModel.FaultContractAttribute> 向协定添加新的错误，或者从协定中移除现有错误。  
  
### <a name="service-contract-libraries"></a>服务协定库  

 组织可以有协定库，协定在这里发布到中心储存库，然后服务实现者从该储存库实现协定。 在此情况下，在将服务协定发布到储存库时，您无法控制谁将创建实现此协定的服务。 因此，在服务协定发布之后，就不能进行修改，这实际上是使协定不可变。 WCF 支持协定继承，该继承可用于创建新的协定来扩展现有的协定。 若要使用此功能，请定义一个继承自旧服务协定接口的新服务协定接口，然后向新接口添加方法。 然后将实现旧协定的服务更改为实现新协定，并将“versionOld”终结点定义更改为使用新协定。 对于“versionOld”客户端，终结点将仍然表现为公开“versionOld”协定；而对于“versionNew”客户端，终结点将表现为公开“versionNew”协定。  
  
## <a name="address-and-binding-versioning"></a>地址和绑定版本管理  

 对终结点地址和绑定的更改是中断性更改，除非客户端能够动态发现新的终结点地址或绑定。 实现此功能的一种机制是使用通用发现、描述和集成 (UDDI) 注册表以及 UDDI 调用模式，在此机制下，客户端尝试与终结点进行通信，并在失败时查询已知的 UDDI 注册表，以获得当前终结点元数据。 然后，客户端使用这些元数据中的地址和绑定来与终结点进行通信。 如果此通信成功，客户端将缓存这些地址和绑定信息以备将来使用。  
  
## <a name="routing-service-and-versioning"></a>路由服务和版本管理  

 如果对服务所做的更改是中断性更改，并且您需要同时运行该服务的两个或多个不同版本，则可使用 WCF 路由服务将消息路由到适当的服务实例。 WCF 路由服务使用基于内容的路由，换句话说，该服务使用消息中的信息来确定将消息路由到何处。 有关 WCF 路由服务的详细信息，请参阅 [路由服务](./feature-details/routing-service.md)。 有关如何使用 WCF 路由服务进行服务版本控制的示例，请参阅 [如何：服务版本控制](./feature-details/how-to-service-versioning.md)。  
  
## <a name="appendix"></a>附录  

 在需要严格版本管理时，通常的数据协定版本管理准则是将数据协定视为不可变，并在需要更改时创建新的协定。 对于每个新的数据协定，需要分别创建一个新类，因此需要一种机制来避免不得不获得按照旧数据协定类编写的现有代码，并按照新数据协定类重新编写代码。  
  
 这样的一种机制是使用接口来定义每个数据协定的成员，并按照这些接口，而不是实现这些接口的数据协定类来编写内部实现代码。 某个服务的版本 1 的以下代码显示了一个 `IPurchaseOrderV1` 接口和一个 `PurchaseOrderV1`：  
  
```csharp  
public interface IPurchaseOrderV1  
{  
    string OrderId { get; set; }  
    string CustomerId { get; set; }  
}  
  
[DataContract(  
Name = "PurchaseOrder",  
Namespace = "http://examples.microsoft.com/WCF/2005/10/PurchaseOrder")]  
public class PurchaseOrderV1 : IPurchaseOrderV1  
{  
    [DataMember(...)]  
    public string OrderId {...}  
    [DataMember(...)]  
    public string CustomerId {...}  
}  
```  
  
 该服务协定的操作将按照 `PurchaseOrderV1` 编写，而实际的业务逻辑将按照 `IPurchaseOrderV1` 编写。 然后，在版本 2 中，将有一个新的 `IPurchaseOrderV2` 接口和一个新的 `PurchaseOrderV2` 类，如下面代码中所示：  
  
```csharp
public interface IPurchaseOrderV2  
{  
    DateTime OrderDate { get; set; }  
}

[DataContract(
Name = "PurchaseOrder",  
Namespace = "http://examples.microsoft.com/WCF/2006/02/PurchaseOrder")]  
public class PurchaseOrderV2 : IPurchaseOrderV1, IPurchaseOrderV2  
{  
    [DataMember(...)]  
    public string OrderId {...}  
    [DataMember(...)]  
    public string CustomerId {...}  
    [DataMember(...)]  
    public DateTime OrderDate { ... }  
}  
```  
  
 该服务协定将更新为包含按照 `PurchaseOrderV2` 编写的新操作。 按照 `IPurchaseOrderV1` 编写的现有业务逻辑对于 `PurchaseOrderV2` 仍然可用，并且需要 `OrderDate` 属性的新业务逻辑将按照 `IPurchaseOrderV2` 编写。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A>
- <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A>
- <xref:System.Runtime.Serialization.IExtensibleDataObject>
- <xref:System.Runtime.Serialization.ExtensionDataObject>
- <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A>
- <xref:System.Xml.Serialization.XmlSerializer>
- [数据协定等效性](./feature-details/data-contract-equivalence.md)
- [版本容错序列化回调](./feature-details/version-tolerant-serialization-callbacks.md)
