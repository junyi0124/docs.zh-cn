---
title: 设计服务协定
description: 了解服务约定，包括如何在 WCF 编程中创建服务协定、可用的操作和数据类型以及服务协定的其他方面。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF]
ms.assetid: 8e89cbb9-ac84-4f0d-85ef-0eb6be0022fd
ms.openlocfilehash: 11d2019023c7389d27607c93b920946837b5c365
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294817"
---
# <a name="designing-service-contracts"></a>设计服务协定

本主题介绍什么是服务协定、如何定义服务协定、可用的操作（以及基础消息交换的含义）、使用的数据类型以及可帮助您设计能满足方案需求的操作的其他问题。  
  
## <a name="creating-a-service-contract"></a>创建服务协定  

 服务公开一系列操作。 在 Windows Communication Foundation (WCF) 应用程序中，通过创建方法并使用特性标记该方法来定义操作 <xref:System.ServiceModel.OperationContractAttribute> 。 然后，若要创建服务协定，需要将操作组合到一起，具体方法是在使用 <xref:System.ServiceModel.ServiceContractAttribute> 属性标记的接口中声明这些操作，或在使用同一属性进行标记的类中定义它们。  (有关基本示例，请参阅 [如何：定义服务协定](how-to-define-a-wcf-service-contract.md)。 )   
  
 没有属性的任何方法 <xref:System.ServiceModel.OperationContractAttribute> 都不是服务操作，并且不是由 WCF 服务公开的。  
  
 本主题介绍设计服务协定时的以下决策要点：  
  
- 是否使用类或接口。  
  
- 如何指定要交换的数据类型。  
  
- 您可以使用的交换模式类型。  
  
- 是否可以将显式安全要求作为协定的一部分。  
  
- 对操作输入和输出的限制。  
  
## <a name="classes-or-interfaces"></a>类或接口  

 类和接口都表示一组功能，因此，这两个接口都可用于定义 WCF 服务协定。 但是，建议您使用接口，因为接口可以直接对服务协定建模。 如果不经过实现，接口的作用只是根据特定签名对一组方法进行定义。 实现服务协定接口，并已实现 WCF 服务。  
  
 服务协定接口具有托管接口的所有优点：  
  
- 服务协定接口可以扩展任何数量的其他服务协定接口。  
  
- 一个类可以通过实现服务协定接口来实现任意数量的服务协定。  
  
- 您可以通过更改接口实现来修改服务协定的实现，而让服务协定保持不变。  
  
- 您可以通过实现旧接口和新接口来确定服务的版本。 老客户端连接到原始版本，而新客户端则可以连接到较新的版本。  
  
> [!NOTE]
> 从其他服务协定接口中继承时，不能重写操作属性，例如名称或命名空间。 如果试图执行该操作，应在当前服务协定中创建新操作。  
  
 有关使用接口创建服务协定的示例，请参阅 [如何：使用协定接口创建服务](./feature-details/how-to-create-a-service-with-a-contract-interface.md)。  
  
 不过，您可以使用类来定义服务协定，并同时实现该协定。 可以通过直接向类和类上的方法分别应用 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.OperationContractAttribute> 来创建服务，这种方法的优点是快速且简便。 缺点是托管类不支持多个继承，因此，一次只能实现一个服务协定。 此外，对类或方法签名的任何修改都将修改该服务的公开协定，这可以防止未进行修改的客户端使用您的服务。 有关详细信息，请参阅 [实现服务协定](implementing-service-contracts.md)。  
  
 有关使用类创建服务协定并同时实现该协定的示例，请参阅 [如何：创建具有协定类的服务](./feature-details/how-to-create-a-wcf-contract-with-a-class.md)。  
  
 此时，您应了解使用接口定义服务协定与使用类定义服务协定之间的区别。 下一步将确定可在服务及其客户端之间往返传递的数据。  
  
## <a name="parameters-and-return-values"></a>参数和返回值  

 每个操作都有一个返回值和一个参数，即使它们为 `void`。 您可以使用局部方法将对对象的引用从一个对象传递到另一个对象，但与局部方法不同的是，服务操作不会传递对对象的引用， 它们传递的只是对象的副本。  
  
 这一点很重要，这是因为参数或返回值中使用的每个类型都必须是可序列化的，换言之，该类型的对象必须能够转换为字节流，并能够从字节流转换为对象。  
  
 默认情况下，基元类型是可序列化的，.NET Framework 中的很多类型都是可序列化的。  
  
> [!NOTE]
> 操作签名中的参数名称值是协定的一部分且区分大小写。 如果要在本地使用相同的参数名称，但是要在已发布的元数据中修改名称，请参见 <xref:System.ServiceModel.MessageParameterAttribute?displayProperty=nameWithType>。  
  
#### <a name="data-contracts"></a>数据协定  

 面向服务的应用程序（如 Windows Communication Foundation (WCF) 应用程序）旨在与 Microsoft 和非 Microsoft 平台上的最大数目的客户端应用程序进行互操作。 为了获得最大可能的互操作性，建议您使用 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性对您的类型进行标记，以创建数据协定。数据协定是服务协定的一部分，用于描述您的服务操作交换的数据。  
  
 数据协定是可选的样式协定：除非您显式应用数据协定属性，否则不会序列化任何类型或数据成员。 数据协定与托管代码的访问范围无关：可以对私有数据成员进行序列化，并将其发送到其他位置，以便可以公开访问它们。  (有关数据协定的基本示例，请参阅 [如何：为类或结构创建基本数据协定](./feature-details/how-to-create-a-basic-data-contract-for-a-class-or-structure.md)。 ) WCF 处理基础 SOAP 消息的定义，这些消息启用该操作的功能，并将您的数据类型序列化到消息的正文中。 只要数据类型可序列化，您就无需在设计操作时考虑基础消息交换基础结构。  
  
 尽管典型的 WCF 应用程序使用 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性为操作创建数据协定，但你可以使用其他序列化机制。 标准 <xref:System.Runtime.Serialization.ISerializable>, <xref:System.SerializableAttribute> 和 <xref:System.Xml.Serialization.IXmlSerializable> 机制都可用于处理数据类型到基础 SOAP 消息的序列化，这些消息可将数据类型从一个应用程序带到另一个应用程序。 如果您的数据类型需要特别支持，您可以采用多个序列化策略。 有关在 WCF 应用程序中序列化数据类型的选项的详细信息，请参阅 [在服务协定中指定数据传输](./feature-details/specifying-data-transfer-in-service-contracts.md)。  
  
#### <a name="mapping-parameters-and-return-values-to-message-exchanges"></a>将参数和返回值映射到消息交换  

 除了应用程序支持特定标准安全、事务和与会话相关的功能时所需的数据之外，对应用程序数据进行往返传输的 SOAP 消息的基础交换还支持服务操作。 因为在这种情况下，服务操作的签名将指定特定的基础 *消息交换模式* (可以支持数据传输和操作所需的功能的 MEP) 。 可以在 WCF 编程模型中指定三种模式：请求/答复、单向和双工消息模式。  
  
##### <a name="requestreply"></a>请求/答复  

 通过请求/答复模式，请求发送方（客户端应用程序）将接收与请求相关的答复。 这是默认的 MEP，因为它既支持传入操作（一个或多个参数传递到该操作中），也支持将返回值传回给调用方。 例如，下面的 C# 代码示例演示一个基本的服务操作，即先接收一个字符串，再返回一个字符串。  
  
```csharp  
[OperationContractAttribute]  
string Hello(string greeting);  
```  
  
 下面是等效的 Visual Basic 代码。  
  
```vb  
<OperationContractAttribute()>  
Function Hello (ByVal greeting As String) As String  
```  
  
 此操作签名指示基础消息交换的形式。 如果没有相关关系，WCF 将无法确定返回值所需的操作。  
  
 请注意，除非指定其他基础消息模式，甚至 `void` 在 Visual Basic) 中返回 (的服务操作 `Nothing` 都是请求/答复消息交换。 您操作的结果是：除非客户端异步调用操作，否则客户端将停止处理，直到收到返回消息，即使该消息正常情况下为空时也是如此。 下面的 C# 代码示例演示的操作在客户端收到空的响应消息后才返回值。  
  
```csharp  
[OperationContractAttribute]  
void Hello(string greeting);  
```  
  
 下面是等效的 Visual Basic 代码。  
  
```vb  
<OperationContractAttribute()>  
Sub Hello (ByVal greeting As String)  
```  
  
 如果执行操作需要很长的时间，则上面的示例会降低客户端性能和响应能力，但是，即使在请求/答复操作返回 `void` 时，这种操作仍有优势。 最明显的优势在于，响应消息中可返回 SOAP 错误，这表明可能在通信或处理中发生了一些与服务有关的错误状况。 在服务协定中指定的 SOAP 错误将作为 <xref:System.ServiceModel.FaultException%601> 对象传递到客户端应用程序，其中类型参数是在服务协定中指定的类型。 这样就可以轻松地在 WCF 服务中向客户端通知错误情况。 有关异常、SOAP 错误和错误处理的详细信息，请参阅 [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md)。 若要查看请求/答复服务和客户端的示例，请参阅 [如何：创建 Request-Reply 协定](./feature-details/how-to-create-a-request-reply-contract.md)。 有关请求-答复模式问题的详细信息，请参阅 [请求-答复服务](./feature-details/request-reply-services.md)。  
  
##### <a name="one-way"></a>单向  

 如果 WCF 服务应用程序的客户端不应等待操作完成并且不处理 SOAP 错误，则操作可以指定单向消息模式。 单向操作是：客户端在其中调用操作并在 WCF 将消息写入网络后继续处理。 通常这意味着，除非在出站消息中发送的数据极其庞大，否则客户端几乎立即继续运行（除非发送数据时出错）。 此种类型的消息交换模式支持从客户端到服务应用程序的类似于事件的行为。  
  
 发送一条消息而未接收任何消息的消息交换无法支持指定非 `void` 的返回值的服务操作；在这种情况下，将引发一个 <xref:System.InvalidOperationException> 异常。  
  
 没有返回消息还意味着可能没有返回表明处理或通信中任何错误的 SOAP 错误。 （操作是单向操作而要求双工消息交换模式时，会产生通信错误信息。）  
  
 若要为返回 `void` 的操作指定单向消息交换，请将 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> 属性设置为 `true`，如下面的 C# 代码示例所示。  
  
```csharp  
[OperationContractAttribute(IsOneWay=true)]  
void Hello(string greeting);  
```  
  
 下面是等效的 Visual Basic 代码。  
  
```vb  
<OperationContractAttribute(IsOneWay := True)>  
Sub Hello (ByVal greeting As String)  
```  
  
 此方法与前面的请求/答复示例相同，但是，将 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> 属性设置为 `true` 意味着尽管方法相同，服务操作也不会发送返回消息，而客户端将在出站消息抵达通道层时立即返回。 有关示例，请参阅 [如何：创建 One-Way 协定](./feature-details/how-to-create-a-one-way-contract.md)。 有关单向模式的详细信息，请参阅单向 [服务](./feature-details/one-way-services.md)。  
  
##### <a name="duplex"></a>双工  

 双工模式的特点是，无论使用单向消息发送还是请求/答复消息发送方式，服务和客户端均能够独立地向对方发送消息。 对于必须直接与客户端通信或向消息交换的任意一方提供异步体验（包括类似于事件的行为）的服务来说，这种双向通信形式非常有用。  
  
 由于存在与客户端通信的附加机制，双向模式比请求/答复或单向模式要略为复杂。  
  
 若要设计双工协定，还必须设计回调协定，并将该回调协定的类型分配给标记服务协定的 <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A> 属性 (Attribute) 的 <xref:System.ServiceModel.ServiceContractAttribute> 属性 (Property)。  
  
 若要实现双工模式，您必须创建第二个接口，该接口包含在客户端调用的方法声明。  
  
 有关创建服务和访问该服务的客户端的示例，请参阅 [如何：创建双工协定](./feature-details/how-to-create-a-duplex-contract.md) 和 [如何：使用双工协定访问服务](./feature-details/how-to-access-services-with-a-duplex-contract.md)。 有关工作示例，请参阅 [双工](./samples/duplex.md)。 有关使用双工协定的问题的详细信息，请参阅 [双工服务](./feature-details/duplex-services.md)。  
  
> [!CAUTION]
> 当服务接收双工消息时，它会在该传入消息中查找 `ReplyTo` 元素，以确定要发送答复的位置。 如果用于接收消息的通道不安全，则不受信任的客户端可能使用目标计算机的 `ReplyTo` 发送恶意消息，从而导致该目标计算机发生拒绝服务 (DOS)。  
  
##### <a name="out-and-ref-parameters"></a>Out 和 Ref 参数  

 在大多数情况下，你可以使用 Visual Basic 中的 `in` 参数 (`ByVal`) 和 `out` (Visual Basic `ref` `ByRef`) 中的参数。 由于 `out` 和 `ref` 参数都指示数据是从操作返回的，类似如下的操作签名会指定需要请求/答复操作，即使操作签名返回 `void` 也是如此。  
  
```csharp  
[ServiceContractAttribute]  
public interface IMyContract  
{  
  [OperationContractAttribute]  
  public void PopulateData(ref CustomDataType data);  
}  
```  
  
 下面是等效的 Visual Basic 代码。  
  
```vb  
<ServiceContractAttribute()> _  
Public Interface IMyContract  
  <OperationContractAttribute()> _  
  Public Sub PopulateData(ByRef data As CustomDataType)  
End Interface  
```  
  
 唯一的例外是当您的签名具有特定结构时。 例如，只有当用于声明操作的方法返回 <xref:System.ServiceModel.NetMsmqBinding> 时，才能使用 `void` 绑定与客户端通信；不能有任何输出值，无论它是返回值、`ref`，还是 `out` 参数。  
  
 此外，使用 `out` 或 `ref` 参数要求操作具有基础响应消息，才可以将已修改的对象传回。 如果操作是单向操作，则将在运行时引发 <xref:System.InvalidOperationException> 异常。  
  
### <a name="specify-message-protection-level-on-the-contract"></a>指定协定上的消息保护级别  

 设计协定时，还必须确定实现您的协定的服务的消息保护级别。 仅当消息安全应用于协定终结点中的绑定时，才有必要这么做。 如果绑定将安全关闭（也就是说，如果系统提供的绑定将 <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType> 设置为值 <xref:System.ServiceModel.SecurityMode.None?displayProperty=nameWithType>），则不必确定协定的消息保护级别。 大部分情况下，系统提供的绑定应用的是消息级别的安全，可提供充分的保护级别，你不必考虑每个操作或每条消息的保护级别。  
  
 保护级别是一个值，它指定了支持服务的消息（或消息部分）是进行签名、签名并加密，还是未经签名或加密即发送。 可以在以下多个范围设置保护级别：服务级别、针对特定操作、针对该操作内的消息或消息部分。 在一个范围设置的值会成为比该范围小的范围的默认值（除非显式重写该值）。 如果绑定配置无法提供协定中要求的最小保护级别，则将引发异常。 如果未在协定上显式设置保护级别值，并且绑定具有消息安全，则绑定配置将控制所有消息的保护级别。 这是默认行为。  
  
> [!IMPORTANT]
> 确定是否将协定的各种范围显式设置为小于 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign?displayProperty=nameWithType> 的完全保护级别的过程，通常就是牺牲部分程度的安全性来换取改善性能的过程。 在这种情况下，您的决策必须围绕您的操作以及它们所交换的数据值进行。 有关详细信息，请参阅 [保护服务](securing-services.md)。  
  
 例如，下面的代码示例不会设置协定的 <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> 或 <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel%2A> 属性。  
  
```csharp  
[ServiceContract]  
public interface ISampleService  
{  
  [OperationContractAttribute]  
  public string GetString();  
  
  [OperationContractAttribute]  
  public int GetInt();
}  
```  
  
 下面是等效的 Visual Basic 代码。  
  
```vb  
<ServiceContractAttribute()> _  
Public Interface ISampleService  
  
  <OperationContractAttribute()> _  
  Public Function GetString()As String  
  
  <OperationContractAttribute()> _  
  Public Function GetData() As Integer  
  
End Interface  
```  
  
 当使用默认的 `ISampleService`（其默认的 <xref:System.ServiceModel.WSHttpBinding> 是 <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType>）在终结点中与 <xref:System.ServiceModel.SecurityMode.Message> 实现交互时，将对所有消息加密并签名，因为这是默认的保护级别。 但是，当 `ISampleService` 服务与默认 <xref:System.ServiceModel.BasicHttpBinding>（其默认的 <xref:System.ServiceModel.SecurityMode> 是 <xref:System.ServiceModel.SecurityMode.None>）一起使用时，所有消息都将以文本的形式发送，这是因为此绑定没有安全性，因此将忽略保护级别（也就是说，不会对消息加密和签名）。 如果 <xref:System.ServiceModel.SecurityMode> 更改为 <xref:System.ServiceModel.SecurityMode.Message>，则将对这些消息加密并签名（因为它现在将成为绑定的默认保护级别）。  
  
 如果要显式指定或调整协定的保护要求，请将 <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> 属性（或较小范围的任意 `ProtectionLevel` 属性）设置为您的服务协定所要求的级别。 在这种情况下，使用显式设置要求绑定在所用的最小范围内支持该设置。 例如，下面的代码示例为 <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel%2A> 操作显式指定一个 `GetGuid` 值。  
  
```csharp  
[ServiceContract]  
public interface IExplicitProtectionLevelSampleService  
{  
  [OperationContractAttribute]  
  public string GetString();  
  
  [OperationContractAttribute(ProtectionLevel=ProtectionLevel.None)]  
  public int GetInt();
  [OperationContractAttribute(ProtectionLevel=ProtectionLevel.EncryptAndSign)]  
  public int GetGuid();
}  
```  
  
 下面是等效的 Visual Basic 代码。  
  
```vb  
<ServiceContract()> _
Public Interface IExplicitProtectionLevelSampleService
    <OperationContract()> _
    Public Function GetString() As String
    End Function
  
    <OperationContract(ProtectionLevel := ProtectionLevel.None)> _
    Public Function GetInt() As Integer
    End Function
  
    <OperationContractAttribute(ProtectionLevel := ProtectionLevel.EncryptAndSign)> _
    Public Function GetGuid() As Integer
    End Function
  
End Interface  
```  
  
 实现此 `IExplicitProtectionLevelSampleService` 协定并且具有使用默认 <xref:System.ServiceModel.WSHttpBinding>（其默认的 <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType> 是 <xref:System.ServiceModel.SecurityMode.Message>）的终结点的服务具有以下行为：  
  
- 对 `GetString` 操作消息加密并签名。  
  
- `GetInt` 操作消息以未加密且未签名文本（即纯文本）的形式发送。  
  
- `GetGuid` 操作 <xref:System.Guid?displayProperty=nameWithType> 将在一条已加密且签名的消息中返回。  
  
 有关保护级别以及如何使用它们的详细信息，请参阅 [了解保护级别](understanding-protection-level.md)。 有关安全性的详细信息，请参阅 [保护服务](securing-services.md)。  
  
##### <a name="other-operation-signature-requirements"></a>其他操作签名要求  

 某些应用程序功能要求特定种类的操作签名。 例如，<xref:System.ServiceModel.NetMsmqBinding> 绑定支持持久性服务和客户端，即应用程序可以在通信期间重新启动，并在其停止的位置处拾取，不会遗漏任何消息。  (有关详细信息，请参阅 [WCF 中的队列](./feature-details/queues-in-wcf.md)。 ) 不过，耐用操作只能 `in` 使用一个参数并且没有返回值。  
  
 另一个示例是在操作中使用 <xref:System.IO.Stream> 类型。 由于 <xref:System.IO.Stream> 参数包括整个消息正文，如果输入或输出（也就是 `ref` 参数、`out` 参数或返回值）的类型为 <xref:System.IO.Stream>，则它必须是在操作中指定的唯一输入或输出。 此外，参数或返回类型必须是 <xref:System.IO.Stream>, <xref:System.ServiceModel.Channels.Message?displayProperty=nameWithType> 或 <xref:System.Xml.Serialization.IXmlSerializable?displayProperty=nameWithType>。 有关流的详细信息，请参阅 [大数据和流式处理](./feature-details/large-data-and-streaming.md)。  
  
##### <a name="names-namespaces-and-obfuscation"></a>名称、命名空间和模糊处理  

 在将协定转换为 WSDL 以及创建和发送协定消息时，协定与操作的定义中的 .NET 类型的名称和命名空间意义重大。 因此，强烈建议使用所有支持协定属性 (Attribute)（如 `Name`、`Namespace`、<xref:System.ServiceModel.ServiceContractAttribute>、<xref:System.ServiceModel.OperationContractAttribute> 和其他协定属性 (Attribute)）的 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (Property) 显式设置服务协定名称和命名空间。  
  
 这样做的一个原因在于，如果未显式设置名称和命名空间，则在程序集上使用 IL 混淆处理时会改变协定类型名称和命名空间，并产生已修改的 WSDL 以及通常会失败的网络交换。 如果未显式设置协定名称和命名空间，但确实想使用混淆处理，请使用 <xref:System.Reflection.ObfuscationAttribute> 和 <xref:System.Reflection.ObfuscateAssemblyAttribute> 属性，以防止修改协定类型名称和命名空间。  
  
## <a name="see-also"></a>另请参阅

- [如何：创建请求-答复协定](./feature-details/how-to-create-a-request-reply-contract.md)
- [如何：创建单向协定](./feature-details/how-to-create-a-one-way-contract.md)
- [如何：创建双工协定](./feature-details/how-to-create-a-duplex-contract.md)
- [在服务协定中指定数据传输](./feature-details/specifying-data-transfer-in-service-contracts.md)
- [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md)
- [使用会话](using-sessions.md)
- [同步和异步操作](synchronous-and-asynchronous-operations.md)
- [Reliable Services](reliable-services.md)
- [服务和事务](services-and-transactions.md)
