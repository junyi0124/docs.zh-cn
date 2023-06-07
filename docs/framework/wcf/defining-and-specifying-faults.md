---
title: 定义和指定错误
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- handling faults [WCF], specifying
- handling faults [WCF], defining
ms.assetid: c00c84f1-962d-46a7-b07f-ebc4f80fbfc1
ms.openlocfilehash: 67096c4531d13bc66f584c09c458c0a5a2a41c6b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96260898"
---
# <a name="defining-and-specifying-faults"></a>定义和指定错误

SOAP 错误可将错误情况信息从服务传达到客户端，在双工情况下，还可以以互操作方式从客户端传达到服务。 此主题讨论何时并且如何自定义错误内容并指定可以返回错误的操作。 有关服务或双工客户端如何发送这些错误以及客户端或服务应用程序如何处理这些错误的详细信息，请参阅 [发送和接收错误](sending-and-receiving-faults.md)。 有关 Windows Communication Foundation (WCF) 应用程序中的错误处理的概述，请参阅在 [协定和服务中指定和处理](specifying-and-handling-faults-in-contracts-and-services.md)错误。  
  
## <a name="overview"></a>概述  

 声明的 SOAP 错误是指其中的某个操作具有 <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType> 的错误，该属性指定自定义 SOAP 错误类型。 未声明的 SOAP 错误是指那些未在相应操作的协定中指定的错误。 本主题帮助您识别这些错误情况并为您的服务创建一个错误协定，当收到自定义 SOAP 错误时，客户端可以用它正确地处理这些错误情况。 基本任务依次为：  
  
1. 定义服务的客户端应该知道的错误情况。  
  
2. 为这些错误情况定义 SOAP 错误的自定义内容。  
  
3. 标记您的操作，以便以 WSDL 向客户端公开这些操作引发的特定 SOAP 错误。  
  
### <a name="defining-error-conditions-that-clients-should-know-about"></a>定义客户端应该知道的错误情况  

 SOAP 错误是公开描述的、传达特定操作的错误信息的消息。 由于它们是同其他操作消息一起以 WSDL 描述的，因此客户端应该知道它们且希望在调用某个操作时处理这样的错误。 但是，因为 WCF 服务是用托管代码编写的，所以，决定将托管代码中的哪些错误条件转换为错误并返回给客户端，这样就可以将服务中的错误情况和 bug 与客户端的正式错误会话隔开。  
  
 例如，下面的代码示例演示一个采用两个整数并返回另一个整数的操作。 这里可引发若干个异常，因此设计错误协定时，必须确定哪些是客户端最重要的错误情况。 在这种情况下，服务应该删除 <xref:System.DivideByZeroException?displayProperty=nameWithType> 异常。  
  
```csharp  
[ServiceContract]  
public class CalculatorService  
{  
    [OperationContract]
    int Divide(int a, int b)  
    {  
      if (b==0) throw new Exception("Division by zero!");  
      return a/b;  
    }  
}  
```  
  
```vb
<ServiceContract> _
Public Class CalculatorService
    <OperationContract> _
    Public Function Divide(a As Integer, b As Integer) As Integer
        If b = 0 Then Throw New DivideByZeroException("Division by zero!")
        Return a / b
    End Function
End Class
```
  
 在上面的示例中，该操作可返回一个特定的除以零的自定义 SOAP 错误、可返回一个特定于数学运算但包含特定的除以零信息的自定义错误、可为若干个不同错误情况返回多个错误，或根本不返回 SOAP 错误。  
  
### <a name="define-the-content-of-error-conditions"></a>定义错误情况的内容  

 将某个错误情况标识为可有效地返回一个自定义 SOAP 错误后，下一步是定义该错误的内容并确保内容结构可以被序列化。 上面的代码示例演示的是特定于 `Divide` 运算的一个错误，如果 `Calculator` 服务上还存在其他运算，则单个自定义 SOAP 错误可通知客户端所有计算器错误情况，包括 `Divide`。 下面的代码示例演示如何创建一个自定义 SOAP 错误 `MathFault`，它可以报告使用所有数学运算（包括 `Divide`）时产生的错误。 当该类可以指定一个操作（`Operation` 属性）和一个描述问题的值（`ProblemType` 属性）时，该类和这些属性必须被序列化以便以自定义 SOAP 错误的形式传输到客户端。 因此，使用 <xref:System.Runtime.Serialization.DataContractAttribute?displayProperty=nameWithType> 和 <xref:System.Runtime.Serialization.DataMemberAttribute?displayProperty=nameWithType> 属性 (Attribute)，以使该类型及其属性 (Property) 可序列化和尽可能可互操作。  
  
 [!code-csharp[Faults#2](../../../samples/snippets/csharp/VS_Snippets_CFX/faults/cs/service.cs#2)]
 [!code-vb[Faults#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faults/vb/service.vb#2)]  
  
 有关如何确保数据可序列化的详细信息，请参阅 [在服务协定中指定数据传输](./feature-details/specifying-data-transfer-in-service-contracts.md)。 有关提供的序列化支持的列表 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> ，请参阅 [数据协定序列化程序支持的类型](./feature-details/types-supported-by-the-data-contract-serializer.md)。  
  
### <a name="mark-operations-to-establish-the-fault-contract"></a>标记操作以建立错误协定  

 定义了一个将作为自定义 SOAP 错误的一部分返回的可序列化数据结构后，最后一步是将操作协定标记为引发该类型的一个 SOAP 错误。 为此，可以使用 <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType> 属性并传递构造的自定义数据类型的类型。 下面的代码示例演示如何使用 <xref:System.ServiceModel.FaultContractAttribute> 属性将 `Divide` 运算指定为可以返回一个 `MathFault` 类型的 SOAP 错误。 现在也可以将其他基于数学的运算指定为可以返回 `MathFault`。  
  
 [!code-csharp[Faults#1](../../../samples/snippets/csharp/VS_Snippets_CFX/faults/cs/service.cs#1)]
 [!code-vb[Faults#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faults/vb/service.vb#1)]  
  
 可以将某个操作指定为返回一个以上的自定义错误，方法为使用一个以上的 <xref:System.ServiceModel.FaultContractAttribute> 属性标记该操作。  
  
 下一步是在操作实现中实现错误协定，如 [发送和接收故障](sending-and-receiving-faults.md)主题中所述。  
  
#### <a name="soap-wsdl-and-interoperability-considerations"></a>SOAP、WSDL 和互操作性的注意事项  

 在某些情况下，特别是与其他平台进行互操作时，控制错误在 SOAP 消息中的显示方式或在 WSDL 元数据中对它的描述方式可能会十分重要。  
  
 <xref:System.ServiceModel.FaultContractAttribute> 属性 (Attribute) 有一个 <xref:System.ServiceModel.FaultContractAttribute.Name%2A> 属性 (Property)，可以控制在元数据中为该错误生成的 WSDL 错误元素名称。  
  
 根据 SOAP 标准，一个错误可有一个 `Action`、一个 `Code`，以及一个 `Reason`。 `Action` 受 <xref:System.ServiceModel.FaultContractAttribute.Action%2A> 属性控制。 <xref:System.ServiceModel.FaultException.Code%2A> 属性和 <xref:System.ServiceModel.FaultException.Reason%2A> 属性是 <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> 类的两个属性，该类是泛型 <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType> 的父类。 `Code` 属性包含一个 <xref:System.ServiceModel.FaultCode.SubCode%2A> 成员。  
  
 在访问生成错误的非服务时，存在某些限制。 WCF 仅支持架构描述并且与数据协定兼容的详细信息类型的错误。 例如，如上所述，WCF 不支持在其详细信息类型中使用 XML 属性的错误，或在详细信息部分中具有多个顶级元素的错误。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.FaultContractAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md)
- [发送和接收错误](sending-and-receiving-faults.md)
- [如何：在服务协定中声明错误](how-to-declare-faults-in-service-contracts.md)
- [了解保护级别](understanding-protection-level.md)
- [如何：设置 ProtectionLevel 属性](how-to-set-the-protectionlevel-property.md)
- [在服务协定中指定数据传输](./feature-details/specifying-data-transfer-in-service-contracts.md)
