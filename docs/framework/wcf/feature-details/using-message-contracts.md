---
title: 使用消息约定
description: 了解如何使用消息协定属性在 WFC 中指定 SOAP 消息的结构。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- message contracts [WCF]
ms.assetid: 1e19c64a-ae84-4c2f-9155-91c54a77c249
ms.openlocfilehash: 39838ac219f095b727ad953158d54da2682a09fa
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282010"
---
# <a name="using-message-contracts"></a>使用消息约定

通常，当构建 Windows Communication Foundation (WCF) 应用程序时，开发人员会密切关注数据结构和序列化问题，而无需考虑在其中携带数据的消息的结构。 对于这些应用程序，为参数或返回值创建数据协定的过程很简单。  (有关详细信息，请参阅 [在服务协定中指定数据传输](specifying-data-transfer-in-service-contracts.md)。 )   
  
 但是，有时完全控制 SOAP 消息的结构与控制其内容一样重要。 当必须提供互操作性或需要在消息或消息部分级别特别控制安全问题时，更是如此。 在这些情况下，你可以创建一个 *消息协定* ，使你能够指定所需的精确 SOAP 消息的结构。  
  
 本主题讨论如何使用各种消息协定属性为操作创建特定消息协定。  
  
## <a name="using-message-contracts-in-operations"></a>在操作中使用消息协定  

 WCF 支持在远程过程调用上建模的操作 *(RPC) 样式* 或 *消息传递样式*。 在 RPC 样式的操作中，可以使用任何可序列化的类型并可以访问可用于本地调用的功能，如多个参数以及 `ref` 和 `out` 参数。 在此样式中，所选序列化的格式控制基础消息中的数据结构，WCF 运行时创建消息来支持该操作。 这使对 SOAP 和 SOAP 消息不熟悉的开发人员能够快速容易地创建和使用服务应用程序。  
  
 下面的代码示例演示在 RPC 样式上建模的服务操作。  
  
```csharp  
[OperationContract]  
public BankingTransactionResponse PostBankingTransaction(BankingTransaction bt);  
```  
  
 通常，定义消息的架构时使用数据协定就足够了。 例如，在前面的示例中，如果 `BankingTransaction` 和 `BankingTransactionResponse` 具有定义基础 SOAP 消息内容的数据协定，则对于大多数应用程序就足够了。 有关数据协定的详细信息，请参阅 [使用数据协定](using-data-contracts.md)。  
  
 但是，有时需要精确控制 SOAP 消息的结构如何通过网络进行传输。 对于这种情况，最常见的方案是插入自定义 SOAP 标头。 另一种常见方案是定义消息头和正文的安全属性，也就是说，确定是否对这些元素进行数字签名和加密。 最后，一些第三方 SOAP 堆栈要求消息采用特定的格式。 消息样式的操作可提供这种控制。  
  
 消息样式的操作最多具有一个参数和一个返回值，其中参数和返回值的类型都是消息类型；也就是说，这两种类型可直接序列化为指定的 SOAP 消息结构。 这可以是用 <xref:System.ServiceModel.MessageContractAttribute> 标记的任何类型或 <xref:System.ServiceModel.Channels.Message> 类型。 下面的代码示例演示类似于前面 RPC 样式的操作，但此操作使用消息样式。  
  
 例如，如果 `BankingTransaction` 和 `BankingTransactionResponse` 都是作为消息协定的类型，则以下操作中的代码有效。  
  
```csharp  
[OperationContract]  
BankingTransactionResponse Process(BankingTransaction bt);  
[OperationContract]  
void Store(BankingTransaction bt);  
[OperationContract]  
BankingTransactionResponse GetResponse();  
```  
  
 但下面的代码无效。  
  
```csharp  
[OperationContract]  
bool Validate(BankingTransaction bt);  
// Invalid, the return type is not a message contract.  
[OperationContract]  
void Reconcile(BankingTransaction bt1, BankingTransaction bt2);  
// Invalid, there is more than one parameter.  
```  
  
 对于涉及消息协定类型且不符合有效模式之一的任何操作会引发异常。 当然，不涉及消息协定类型的操作不受这些限制的约束。  
  
 如果一个类型既有消息协定又有数据协定，则在操作中使用此类型时只考虑其消息协定。  
  
## <a name="defining-message-contracts"></a>定义消息协定  

 若要为某一类型定义消息协定（即定义该类型和 SOAP 信封之间的映射），请对该类型应用 <xref:System.ServiceModel.MessageContractAttribute>。 然后对该类型中要成为 SOAP 标头的成员应用 <xref:System.ServiceModel.MessageHeaderAttribute>，并对要成为消息的 SOAP 正文部分的成员应用 <xref:System.ServiceModel.MessageBodyMemberAttribute>。  
  
 下面的代码提供了一个使用消息协定的示例。  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageHeader] public DateTime transactionDate;  
  [MessageBodyMember] private Account sourceAccount;  
  [MessageBodyMember] private Account targetAccount;  
  [MessageBodyMember] public int amount;  
}  
```  
  
 使用此类型作为操作参数时，会生成以下的 SOAP 信封：  
  
```xml  
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
  <s:Header>  
    <h:operation xmlns:h="http://tempuri.org/" xmlns="http://tempuri.org/">Deposit</h:operation>  
    <h:transactionDate xmlns:h="http://tempuri.org/" xmlns="http://tempuri.org/">2012-02-16T16:10:00</h:transactionDate>  
  </s:Header>  
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <BankingTransaction xmlns="http://tempuri.org/">  
      <amount>0</amount>  
      <sourceAccount xsi:nil="true"/>  
      <targetAccount xsi:nil="true"/>  
    </BankingTransaction>  
  </s:Body>  
</s:Envelope>  
```  
  
 请注意，`operation` 和 `transactionDate` 显示为 SOAP 标头，而 SOAP 正文由包装元素 `BankingTransaction` 组成，其中包括 `sourceAccount`、`targetAccount` 和 `amount`。  
  
 可以对所有字段、属性和事件应用 <xref:System.ServiceModel.MessageHeaderAttribute> 和 <xref:System.ServiceModel.MessageBodyMemberAttribute>，而不管这些字段、属性和事件是公用的、私有的、受保护的还是内部的。  
  
 <xref:System.ServiceModel.MessageContractAttribute> 允许您指定 WrapperName 和 WrapperNamespace 特性，这些特性控制 SOAP 消息的正文中包装元素的名称。 默认情况下，消息协定类型的名称用于包装，而在其中定义消息协定的命名空间 `http://tempuri.org/` 用作默认的命名空间。  
  
> [!NOTE]
> 消息协定中会忽略 <xref:System.Runtime.Serialization.KnownTypeAttribute> 属性。 如果需要 <xref:System.Runtime.Serialization.KnownTypeAttribute>，可以将其放在使用所述消息协定的操作上。  
  
## <a name="controlling-header-and-body-part-names-and-namespaces"></a>控制标头和正文部分的名称和命名空间  

 在消息协定的 SOAP 表示形式中，每个标头和正文部分都映射为一个具有名称和命名空间的 XML 元素。  
  
 默认情况下，命名空间与消息加入的服务协定的命名空间相同，名称由应用 <xref:System.ServiceModel.MessageHeaderAttribute> 或 <xref:System.ServiceModel.MessageBodyMemberAttribute> 属性的成员名称确定。  
  
 通过操作 <xref:System.ServiceModel.MessageContractMemberAttribute.Name%2A?displayProperty=nameWithType> 和 <xref:System.ServiceModel.MessageContractMemberAttribute.Namespace%2A?displayProperty=nameWithType>（在 <xref:System.ServiceModel.MessageHeaderAttribute> 和 <xref:System.ServiceModel.MessageBodyMemberAttribute> 特性的父类上）可以更改这些默认值。  
  
 考虑下面代码示例中的类。  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageHeader(Namespace="http://schemas.contoso.com/auditing/2005")] public bool IsAudited;  
  [MessageBodyMember(Name="transactionData")] public BankingTransactionData theData;  
}  
```  
  
 在此示例中，`IsAudited` 标头位于代码中指定的命名空间中，表示 `theData` 成员的正文部分由名为 `transactionData` 的 XML 元素表示。 下面显示了为此消息协定生成的 XML。  
  
```xml  
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
  <s:Header>  
    <h:IsAudited xmlns:h="http://schemas.contoso.com/auditing/2005" xmlns="http://schemas.contoso.com/auditing/2005">false</h:IsAudited>  
    <h:operation xmlns:h="http://tempuri.org/" xmlns="http://tempuri.org/">Deposit</h:operation>  
  </s:Header>  
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <AuditedBankingTransaction xmlns="http://tempuri.org/">  
      <transactionData/>  
    </AuditedBankingTransaction>  
  </s:Body>  
</s:Envelope>  
```  
  
## <a name="controlling-whether-the-soap-body-parts-are-wrapped"></a>控制是否包装 SOAP 正文部分  

 默认情况下，SOAP 正文部分会在包装元素内部进行序列化。 例如，下面的代码演示从 `HelloGreetingMessage` 消息的消息协定中的 <xref:System.ServiceModel.MessageContractAttribute> 类型的名称生成的 `HelloGreetingMessage` 包装元素。  
  
[!code-csharp[MessageHeaderAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/messageheaderattribute/cs/services.cs#3)]
[!code-vb[MessageHeaderAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/messageheaderattribute/vb/services.vb#3)]  
  
 若要取消包装元素，请将 <xref:System.ServiceModel.MessageContractAttribute.IsWrapped%2A> 属性设置为 `false`。 若要控制包装元素的名称和命名空间，请使用 <xref:System.ServiceModel.MessageContractAttribute.WrapperName%2A> 和 <xref:System.ServiceModel.MessageContractAttribute.WrapperNamespace%2A> 属性。  
  
> [!NOTE]
> 消息中具有多个未包装的消息正文部分不符合 WS-I 基本配置文件 1.1 的规定，在设计新消息协定时不建议这样做。 但是，在某些特定的互操作性方案中，必需要具有多个未包装的消息正文部分。 如果要在消息正文中传输多段数据，则建议使用默认（包装）模式。 未包装的消息中具有多个消息头是完全可以的。  
  
## <a name="using-custom-types-inside-message-contracts"></a>在消息协定内部使用自定义类型  

 每个单独的消息头和消息正文部分均使用为消息所使用的服务协定选择的序列化引擎进行序列化（转换为 XML）。 默认序列化引擎  可以显式处理（通过具有 ）或隐式处理（通过作为基元类型而具有 等）具有数据协定的任何类型。 有关详细信息，请参阅 [使用数据协定](using-data-contracts.md)。  
  
 在前面的示例中，`Operation` 和 `BankingTransactionData` 类型必须具有数据约，定且 `transactionDate` 可序列化，因为 <xref:System.DateTime> 是基元类型（因此具有隐式数据协定）。  
  
 不过，也可以切换到另一个序列化引擎 `XmlSerializer`。 如果进行切换，应确保用于消息头和正文部分的所有类型都可以使用 `XmlSerializer` 进行序列化。  
  
## <a name="using-arrays-inside-message-contracts"></a>在消息协定内部使用数组  

 可以采用两种方式在消息协定中使用重复元素的数组。  
  
 第一种方式是直接在数组上使用 <xref:System.ServiceModel.MessageHeaderAttribute> 或 <xref:System.ServiceModel.MessageBodyMemberAttribute>。 在这种情况下，整个数组序列化为一个具有多个子元素的元素（即一个标头或一个正文部分）。 考虑下面示例中的类。  
  
```csharp  
[MessageContract]  
public class BankingDepositLog  
{  
  [MessageHeader] public int numRecords;  
  [MessageHeader] public DepositRecord[] records;  
  [MessageHeader] public int branchID;  
}  
```  
  
 这会使 SOAP 标头类似于下面所示。  
  
```xml  
<BankingDepositLog>  
<numRecords>3</numRecords>  
<records>  
  <DepositRecord>Record1</DepositRecord>  
  <DepositRecord>Record2</DepositRecord>  
  <DepositRecord>Record3</DepositRecord>  
</records>  
<branchID>20643</branchID>  
</BankingDepositLog>  
```  
  
 另一种方式是使用 <xref:System.ServiceModel.MessageHeaderArrayAttribute>。 在这种情况下，每个数组元素都单独序列化，从而使每个数组元素都具有一个标头，类似于下面所示。  
  
```xml  
<numRecords>3</numRecords>  
<records>Record1</records>  
<records>Record2</records>  
<records>Record3</records>  
<branchID>20643</branchID>  
```  
  
 数组项的默认名称是对其应用 <xref:System.ServiceModel.MessageHeaderArrayAttribute> 属性的成员的名称。  
  
 <xref:System.ServiceModel.MessageHeaderArrayAttribute> 属性继承自 <xref:System.ServiceModel.MessageHeaderAttribute>。 它具有与非数组属性相同的一组功能，例如，可以像设置单一标头的顺序、名称和命名空间那样设置标头数组的顺序、名称和命名空间。 在对数组使用 `Order` 属性时，该属性将应用于整个数组。  
  
 可以将 <xref:System.ServiceModel.MessageHeaderArrayAttribute> 只应用于数组，而不应用于集合。  
  
## <a name="using-byte-arrays-in-message-contracts"></a>在消息协定中使用字节数组  

 与非数组属性（<xref:System.ServiceModel.MessageBodyMemberAttribute> 和 <xref:System.ServiceModel.MessageHeaderAttribute>）一起使用时，字节数组不被视为数组，而被视为一种特殊的基元类型，在生成的 XML 中表示为 Base64 编码的数据。  
  
 在将字节数组与数组属性 <xref:System.ServiceModel.MessageHeaderArrayAttribute> 一起使用时，结果取决于正在使用的序列化程序。 如果使用默认序列化程序，会将数组表示为每个字节一个单独项。 但如果选择 `XmlSerializer`（在服务协定上使用 <xref:System.ServiceModel.XmlSerializerFormatAttribute>），则会将字节数组视为 Base64 数据，而不管是使用数组属性还是非数组属性。  
  
## <a name="signing-and-encrypting-parts-of-the-message"></a>对消息部分进行签名和加密  

 消息协定可以指示消息头和/或正文是否应进行数字签名和加密。  
  
 这可以通过在 <xref:System.ServiceModel.MessageContractMemberAttribute.ProtectionLevel%2A?displayProperty=nameWithType> 和 <xref:System.ServiceModel.MessageHeaderAttribute> 特性上设置 <xref:System.ServiceModel.MessageBodyMemberAttribute> 属性来完成。 该属性是 <xref:System.Net.Security.ProtectionLevel?displayProperty=nameWithType> 类型的枚举，可以设置为 <xref:System.Net.Security.ProtectionLevel.None>（不加密或签名）、<xref:System.Net.Security.ProtectionLevel.Sign>（仅数字签名）或 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>（加密并数字签名）。 默认值为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>。  
  
 若要让这些安全功能起作用，必须正确配置绑定和行为。 如果在没有正确配置的情况下使用这些安全功能（例如，在不提供凭据的情况下试图对消息进行签名），则会在验证时引发异常。  
  
 对于消息头，会分别为每个消息头确定其保护级别。  
  
 对于消息正文部分，保护级别可理解为“最低保护级别”。 无论包含几个正文部分，正文都只有一个保护级别。 正文的保护级别由所有正文部分的最高 <xref:System.ServiceModel.MessageContractMemberAttribute.ProtectionLevel%2A> 属性设置确定。 不过，您应该将每个正文部分的保护级别设置为实际要求的最低保护级别。  
  
 考虑下面代码示例中的类。  
  
```csharp  
[MessageContract]  
public class PatientRecord  
{  
   [MessageHeader(ProtectionLevel=None)] public int recordID;  
   [MessageHeader(ProtectionLevel=Sign)] public string patientName;  
   [MessageHeader(ProtectionLevel=EncryptAndSign)] public string SSN;  
   [MessageBodyMember(ProtectionLevel=None)] public string comments;  
   [MessageBodyMember(ProtectionLevel=Sign)] public string diagnosis;  
   [MessageBodyMember(ProtectionLevel=EncryptAndSign)] public string medicalHistory;  
}  
```  
  
 在此示例中，`recordID` 标头未受保护，`patientName` 为 `signed`，`SSN` 进行了加密和签名。 至少有一个正文部分 `medicalHistory` 已应用 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>，因此将对整个消息正文进行加密和签名，即使 comments 和 diagnosis 正文部分指定了较低的保护级别。  
  
## <a name="soap-action"></a>SOAP 操作  

 SOAP 和相关 Web 服务标准定义了一个名为 `Action` 的属性，该属性可用于发送的每个 SOAP 消息。 操作的 <xref:System.ServiceModel.OperationContractAttribute.Action%2A?displayProperty=nameWithType> 和 <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A?displayProperty=nameWithType> 属性控制此属性的值。  
  
## <a name="soap-header-attributes"></a>SOAP 标头属性  

 SOAP 标准定义了下列可存在于标头上的属性：  
  
- `Actor/Role`（在 SOAP 1.1 中为 `Actor`，在 SOAP 1.2 中为 `Role`）  
  
- `MustUnderstand`  
  
- `Relay`  
  
 `Actor` 或 `Role` 属性指定要使用给定标头的节点的统一资源标识符 (URI)。 `MustUnderstand` 属性指定处理标头的节点是否必须理解该标头。 `Relay` 特性指定标头是否要中继到下游节点。 除了 `MustUnderstand` 本主题后面的 "消息协定版本管理" 一节中所述，WCF 不对传入消息执行这些特性的任何处理。 但它允许您根据需要读取和写入这些属性，如下所述。  
  
 发送消息时，默认情况下不会发出这些属性。 您可以采取两种方式更改这一行为。 第一种方式是通过更改 <xref:System.ServiceModel.MessageHeaderAttribute.Actor%2A?displayProperty=nameWithType>、<xref:System.ServiceModel.MessageHeaderAttribute.MustUnderstand%2A?displayProperty=nameWithType> 和 <xref:System.ServiceModel.MessageHeaderAttribute.Relay%2A?displayProperty=nameWithType> 属性，以静态方式将这些特性设置为任何需要的值，如下面的代码示例所示。 （请注意，没有 `Role` 属性；如果使用 SOAP 1.2，则设置 <xref:System.ServiceModel.MessageHeaderAttribute.Actor%2A> 属性会发出 `Role` 特性）。  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader(Actor="http://auditingservice.contoso.com", MustUnderstand=true)] public bool IsAudited;  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public BankingTransactionData theData;  
}  
```  
  
 第二种方式是通过代码以动态方式控制这些属性。 为此，您可以将所需的标头包装在 <xref:System.ServiceModel.MessageHeader%601> 类型中（切勿将此类型与非泛型版本混淆），并与 <xref:System.ServiceModel.MessageHeaderAttribute> 一起使用该类型。 然后，可以在 <xref:System.ServiceModel.MessageHeader%601> 上使用属性来设置 SOAP 特性，如下面的代码示例所示。  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public MessageHeader<bool> IsAudited;  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public BankingTransactionData theData;  
}  
// application code:  
BankingTransaction bt = new BankingTransaction();  
bt.IsAudited = new MessageHeader<bool>();  
bt.IsAudited.Content = false; // Set IsAudited header value to "false"  
bt.IsAudited.Actor="http://auditingservice.contoso.com";  
bt.IsAudited.MustUnderstand=true;  
```  
  
 如果同时使用动态和静态控制机制，则静态设置用作默认设置，但可以在以后使用动态机制重写，如下面的代码所示。  
  
```csharp  
[MessageHeader(MustUnderstand=true)] public MessageHeader<Person> documentApprover;  
// later on in the code:  
BankingTransaction bt = new BankingTransaction();  
bt.documentApprover = new MessageHeader<Person>();  
bt.documentApprover.MustUnderstand = false; // override the static default of 'true'  
```  
  
 允许创建具有动态属性控制的重复标头，如下面的代码所示。  
  
```csharp  
[MessageHeaderArray] public MessageHeader<Person> documentApprovers[];  
```  
  
 在接收端，仅当对类型中的标头使用 <xref:System.ServiceModel.MessageHeader%601> 类时才能读取这些 SOAP 属性。 请检查 `Actor` 类型的标头的 `Relay`、`MustUnderstand` 或 <xref:System.ServiceModel.MessageHeader%601> 属性 (Property) 以发现所接收消息的属性 (Attribute) 设置。  
  
 当接收到消息然后发回该消息时，SOAP 属性设置仅对 <xref:System.ServiceModel.MessageHeader%601> 类型的标头往返一次。  
  
## <a name="order-of-soap-body-parts"></a>SOAP 正文部分的顺序  

 在某些情况下，可能需要控制正文部分的顺序。 默认情况下，正文元素采用字母顺序，但可以通过 <xref:System.ServiceModel.MessageBodyMemberAttribute.Order%2A?displayProperty=nameWithType> 属性进行控制。 此属性具有与 <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A?displayProperty=nameWithType> 属性相同的语义，但在继承方案中的行为除外（在消息协定中，基类型正文成员不排列在派生类型正文成员之前）。 有关详细信息，请参阅 [数据成员顺序](data-member-order.md)。  
  
 在下面的示例中，`amount` 通常排在第一位，因为按字母顺序它排在第一位。 但 <xref:System.ServiceModel.MessageBodyMemberAttribute.Order%2A> 属性将它放在了第三位。  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember(Order=1)] public Account sourceAccount;  
  [MessageBodyMember(Order=2)] public Account targetAccount;  
  [MessageBodyMember(Order=3)] public int amount;  
}  
```  
  
## <a name="message-contract-versioning"></a>消息协定版本管理  

 有时，您可能需要更改消息协定。 例如，应用程序的新版本可能会向消息中添加额外的标头。 在从新版本应用程序向旧版本应用程序发送消息时，系统必须处理额外的标头；同样，反方向操作时系统必须处理缺少的标头。  
  
 下面的规则适用于标头的版本管理：  
  
- WCF 不会将对象移到缺少的标头，相应的成员将保留其默认值。  
  
- WCF 还忽略意外的额外标头。 此规则的一种例外情况是在传入的 SOAP 消息中，额外标头的 `MustUnderstand` 属性设置为 `true`。在这种情况下，由于存在一个无法处理但必须理解的标头，因此会引发异常。  
  
 消息正文具有类似的版本管理规则，即忽略缺少和附加的消息正文部分。  
  
## <a name="inheritance-considerations"></a>继承注意事项  

 消息协定类型可以继承自另一个类型，前提是基类型也具有消息协定。  
  
 在使用继承自其他消息协定类型的消息协定类型创建或访问消息时，下面的规则适用：  
  
- 继承层次结构中的所有消息头集合在一起构成消息头的完整集合。  
  
- 继承层次结构中的所有消息正文部分集合在一起构成消息正文的完整集合。 正文部分按照通常排序规则排列（按 <xref:System.ServiceModel.MessageBodyMemberAttribute.Order%2A?displayProperty=nameWithType> 属性，然后按字母顺序排序），与它们在继承层次结构中的位置无关。 在使用消息协定继承时，强烈建议消息正文部分不要出现在继承树的多个级别上。 如果基类和派生类用相同的名称定义标头或正文部分，则使用最基础的类中的成员来存储该标头或正文部分的值。  
  
 考虑下面代码示例中的类。  
  
```csharp  
[MessageContract]  
public class PersonRecord  
{  
  [MessageHeader(Name="ID")] public int personID;  
  [MessageBodyMember] public string patientName;  
}  
  
[MessageContract]  
public class PatientRecord : PersonRecord  
{  
  [MessageHeader(Name="ID")] public int patientID;  
  [MessageBodyMember] public string diagnosis;  
}  
```  
  
 `PatientRecord` 类描述了一个消息，该消息具有一个名为 `ID` 的标头。 该标头对应于 `personID` 而不对应于 `patientID` 成员，因为选择了最基础的成员。 因此，在这种情况下，`patientID` 字段无用。 消息正文包含 `diagnosis` 元素，后面跟随 `patientName` 元素，因为这是这些元素的字母顺序。 表注意，该示例演示了一种强烈建议不要使用的模式：基消息协定和派生消息协定都具有消息正文部分。  
  
## <a name="wsdl-considerations"></a>WSDL 注意事项  

 在从使用消息协定的服务生成 Web 服务描述语言 (WSDL) 协定时，必须记住生成的 WSDL 中并不会反映所有的消息协定功能。 请考虑以下要点：  
  
- WSDL 无法表示标头数组的概念。 使用 <xref:System.ServiceModel.MessageHeaderArrayAttribute> 创建具有标头数组的消息时，生成的 WSDL 只反映一个标头而不是该数组。  
  
- 生成的 WSDL 文档可能不反映某些保护级别信息。  
  
- 在 WSDL 中生成的消息类型的名称与消息协定类型的类名称相同。  
  
- 在多个操作中使用同一个消息协定时，会在 WSDL 文档中生成多个消息类型。 对于后续使用，会在名称中添加数字“2”、“3”等以使名称唯一。 在导回 WSDL 时，会创建多个消息协定类型，除了名称不同外，这些消息协定类型完全相同。  
  
## <a name="soap-encoding-considerations"></a>SOAP 编码注意事项  

 WCF 允许使用 XML 的旧版 SOAP 编码样式，但是不建议使用它。 使用这种样式时（在应用于服务协定的 `Use` 上将 `Encoded` 属性设置为 <xref:System.ServiceModel.XmlSerializerFormatAttribute?displayProperty=nameWithType>），下面的附加注意事项适用：  
  
- 不支持消息头；这意味着属性 <xref:System.ServiceModel.MessageHeaderAttribute> 和数组属性 <xref:System.ServiceModel.MessageHeaderArrayAttribute> 与 SOAP 编码不兼容。  
  
- 如果未包装消息协定，即如果属性 <xref:System.ServiceModel.MessageContractAttribute.IsWrapped%2A> 设置为 `false`，则消息协定只能具有一个正文部分。  
  
- 请求消息协定的包装元素的名称必须与操作名称匹配。 为此请使用消息协定的 `WrapperName` 属性。  
  
- 响应消息协定的包装元素的名称必须与具有“Response”后缀的操作名称相同。 为此请使用消息协定的 <xref:System.ServiceModel.MessageContractAttribute.WrapperName%2A> 属性。  
  
- SOAP 编码保留对象引用。 例如，考虑下面的代码。  
  
    ```csharp  
    [MessageContract(WrapperName="updateChangeRecord")]  
    public class ChangeRecordRequest  
    {  
      [MessageBodyMember] Person changedBy;  
      [MessageBodyMember] Person changedFrom;  
      [MessageBodyMember] Person changedTo;  
    }  
  
    [MessageContract(WrapperName="updateChangeRecordResponse")]  
    public class ChangeRecordResponse  
    {  
      [MessageBodyMember] Person changedBy;  
      [MessageBodyMember] Person changedFrom;  
      [MessageBodyMember] Person changedTo;  
    }  
  
    // application code:  
    ChangeRecordRequest cr = new ChangeRecordRequest();  
    Person p = new Person("John Doe");  
    cr.changedBy=p;  
    cr.changedFrom=p;  
    cr.changedTo=p;  
    ```  
  
 使用 SOAP 编码序列化消息后，`changedFrom` 和 `changedTo` 不包含其自己的 `p` 副本，而是指向 `changedBy` 元素内的副本。  
  
## <a name="performance-considerations"></a>性能注意事项  

 每个消息头和消息正文部分相互独立地进行序列化。 因此，可以为每个标头和正文部分重新声明相同的命名空间。 为了提高性能，特别是对于消息在网络上的大小，请将多个标头和正文部分合并成一个标头或正文部分。 例如，不要使用下面代码：  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public Account sourceAccount;  
  [MessageBodyMember] public Account targetAccount;  
  [MessageBodyMember] public int amount;  
}  
```  
  
 请使用此代码。  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public OperationDetails details;  
}  
  
[DataContract]  
public class OperationDetails  
{  
  [DataMember] public Account sourceAccount;  
  [DataMember] public Account targetAccount;  
  [DataMember] public int amount;  
}  
```  
  
### <a name="event-based-asynchronous-and-message-contracts"></a>基于事件的异步协定和消息协定  

 基于事件的异步模型设计准则规定，如果返回了多个值，则一个值会作为 `Result` 属性返回，其他值会作为 <xref:System.EventArgs> 对象上的属性返回。 因此产生的结果之一是，如果客户端使用基于事件的异步命令选项导入元数据，且该操作返回多个值，则默认的 <xref:System.EventArgs> 对象将返回一个值作为 `Result` 属性，返回的其余值是 <xref:System.EventArgs> 对象的属性。  
  
 如果要将消息对象作为 `Result` 属性来接收并要使返回的值作为该对象上的属性，请使用 `/messageContract` 命令选项。 这会生成一个签名，该签名会将响应消息作为 `Result` 对象上的 <xref:System.EventArgs> 属性返回。 然后，所有内部返回值就都是响应消息对象的属性了。  
  
## <a name="see-also"></a>请参阅

- [使用数据协定](using-data-contracts.md)
- [设计和实现服务](../designing-and-implementing-services.md)
