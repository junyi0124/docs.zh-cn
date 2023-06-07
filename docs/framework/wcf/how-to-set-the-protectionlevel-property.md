---
title: 如何：设置 ProtectionLevel 属性
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, security
- ProtectionLevel property
ms.assetid: 3d4e8f80-0f9e-4a26-9899-beb6584e78df
ms.openlocfilehash: 359364228c4ab4d5b247f4f42f3ef3391f774197
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236593"
---
# <a name="how-to-set-the-protectionlevel-property"></a>如何：设置 ProtectionLevel 属性

通过应用相应的属性 (Attribute) 并设置该属性 (Property) 可设置保护级别。 可在服务级设置保护以影响每条消息的所有部分，或者可以从方法到消息部分逐级递增地设置保护。 有关属性的详细信息 `ProtectionLevel` ，请参阅 [了解保护级别](understanding-protection-level.md)。  
  
> [!NOTE]
> 只能在代码中，而不是在配置中设置保护级别。  
  
### <a name="to-sign-all-messages-for-a-service"></a>为服务签名所有消息  
  
1. 为服务创建一个接口。  
  
2. 将 <xref:System.ServiceModel.ServiceContractAttribute> 属性 (Attribute) 应用于服务，并将 <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> 属性 (Property) 设置为 <xref:System.Net.Security.ProtectionLevel.Sign>，如下面的代码所示（默认级别为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>）。  
  
     [!code-csharp[C_ProtectionLevel#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#1)]
     [!code-vb[C_ProtectionLevel#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#1)]  
  
### <a name="to-sign-all-message-parts-for-an-operation"></a>为操作签名所有消息部分  
  
1. 为服务创建一个接口，并将 <xref:System.ServiceModel.ServiceContractAttribute> 属性应用于该接口。  
  
2. 向该接口添加一个方法声明。  
  
3. 将 <xref:System.ServiceModel.OperationContractAttribute> 属性 (Attribute) 应用于该方法，并将 <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> 属性 (Property) 设置为 <xref:System.Net.Security.ProtectionLevel.Sign>，如下面的代码所示。  
  
     [!code-csharp[C_ProtectionLevel#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#2)]
     [!code-vb[C_ProtectionLevel#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#2)]  
  
## <a name="protecting-fault-messages"></a>保护错误消息  

 服务上引发的异常可以作为 SOAP 故障发送到客户端。 有关创建强类型错误的详细信息，请参阅 [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md) 和 [如何：在服务协定中声明错误](how-to-declare-faults-in-service-contracts.md)。  
  
#### <a name="to-protect-a-fault-message"></a>保护错误消息  
  
1. 创建一个表示错误消息的类型。 下面的示例创建一个含有两个字段的名为 `MathFault` 的类。  
  
2. 将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于该类型，并将 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性应用于应该序列化的每个字段，如下面的代码所示。  
  
     [!code-csharp[C_ProtectionLevel#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#3)]
     [!code-vb[C_ProtectionLevel#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#3)]  
  
3. 在将返回该错误的接口中，将 <xref:System.ServiceModel.FaultContractAttribute> 属性应用于将返回错误的方法，并将 `detailType` 参数设置为错误类的类型。  
  
4. 同样在构造函数中，将 <xref:System.ServiceModel.FaultContractAttribute.ProtectionLevel%2A> 属性设置为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>，如下面的代码所示。  
  
     [!code-csharp[C_ProtectionLevel#4](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#4)]
     [!code-vb[C_ProtectionLevel#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#4)]  
  
## <a name="protecting-message-parts"></a>保护消息部分  

 使用消息协定来保护消息的各个部分。 有关消息约定的详细信息，请参阅 [使用消息协定](./feature-details/using-message-contracts.md)。  
  
#### <a name="to-protect-a-message-body"></a>保护消息正文  
  
1. 创建一个表示消息的类型。 下面的示例创建一个 `Company` 类，其中有两个字段 `CompanyName` 和 `CompanyID`。  
  
2. 将 <xref:System.ServiceModel.MessageContractAttribute> 属性 (Attribute) 应用于该类，并将 <xref:System.ServiceModel.MessageContractAttribute.ProtectionLevel%2A> 属性 (Property) 设置为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>。  
  
3. 将 <xref:System.ServiceModel.MessageHeaderAttribute> 属性 (Attribute) 应用于将表示为消息头的字段，并将 `ProtectionLevel` 属性 (Property) 设置为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>。  
  
4. 将应用于 <xref:System.ServiceModel.MessageBodyMemberAttribute> 将表示为消息正文的一部分的任何字段，并将 `ProtectionLevel` 属性设置为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> ，如下面的示例中所示。  
  
     [!code-csharp[C_ProtectionLevel#5](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#5)]
     [!code-vb[C_ProtectionLevel#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#5)]  
  
## <a name="example"></a>示例  

 下面的示例设置位于服务中不同位置的多个属性 (Attribute) 类的 `ProtectionLevel` 属性 (Property)。  
  
 [!code-csharp[C_ProtectionLevel#6](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#6)]
 [!code-vb[C_ProtectionLevel#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#6)]  
  
## <a name="compiling-the-code"></a>编译代码  

 下面的代码显示编译该示例代码所必需的命名空间。  
  
 [!code-csharp[C_ProtectionLevel#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_protectionlevel/cs/source.cs#0)]
 [!code-vb[C_ProtectionLevel#0](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_protectionlevel/vb/source.vb#0)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>
- <xref:System.ServiceModel.FaultContractAttribute>
- <xref:System.ServiceModel.MessageContractAttribute>
- <xref:System.ServiceModel.MessageBodyMemberAttribute>
- [了解保护级别](understanding-protection-level.md)
