---
title: 如何：自定义系统提供的绑定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f8b97862-e8bb-470d-8b96-07733c21fe26
ms.openlocfilehash: e3d7cb72bcbdd636530e7861071b73f8a5f38b31
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249223"
---
# <a name="how-to-customize-a-system-provided-binding"></a>如何：自定义系统提供的绑定

Windows Communication Foundation (WCF) 包含多个系统提供的绑定，这些绑定允许您配置基础绑定元素的某些属性，但不是所有属性。 本主题演示如何设置绑定元素上的属性以创建自定义绑定。  
  
 有关如何在不使用系统提供的绑定的情况下直接创建和配置绑定元素的更多信息，请参见 [自定义绑定](custom-bindings.md)。  
  
 有关创建和扩展自定义绑定的详细信息，请参阅 [扩展绑定](extending-bindings.md)。  
  
 在 WCF 中，所有绑定都由 *绑定元素* 组成。 每个绑定元素都是从 <xref:System.ServiceModel.Channels.BindingElement> 类派生的。 系统提供的绑定（如 <xref:System.ServiceModel.BasicHttpBinding>）可创建和配置各自的绑定元素。 本主题演示如何访问和更改这些绑定元素的属性，这些属性不会直接在绑定上公开，具体而言，这些属性不会直接在 <xref:System.ServiceModel.BasicHttpBinding> 类上公开。  
  
 各个绑定元素都包含在由 <xref:System.ServiceModel.Channels.BindingElementCollection> 类表示的集合中，并按如下顺序添加：事务流、可靠会话、安全、复合双工、单向、流安全、消息编码和传输协议。 请注意，并不是每个绑定中都需要所列的所有绑定元素。 用户定义的绑定元素也可以出现在此绑定元素集合中，但必须按前面所述的相同顺序出现。 例如，用户定义的传输协议必须为绑定元素集合中的最后一个元素。  
  
 <xref:System.ServiceModel.BasicHttpBinding> 类包含三个绑定元素：  
  
1. 可选的安全绑定元素，即，与 HTTP 传输协议（消息级安全）一起使用的 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 类，或在传输协议层提供安全性时使用的 <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> 类（这种情况下将使用 HTTPS 传输协议）。  
  
2. 必需的消息编码器绑定元素，即，<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> 或 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>。  
  
3. 必需的传输绑定元素，即，<xref:System.ServiceModel.Channels.HttpTransportBindingElement> 或 <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>。  
  
 在此示例中，我们将创建绑定的一个实例，根据其生成 *自定义绑定* ，检查自定义绑定中的绑定元素，并在找到 HTTP 绑定元素时将其 `KeepAliveEnabled` 属性设置为 `false` 。 由于在 `KeepAliveEnabled` 上未直接公开 `BasicHttpBinding` 属性，因此必须创建一个自定义绑定以向下导航至绑定元素，并设置此属性。  
  
### <a name="to-modify-a-system-provided-binding"></a>修改系统提供的绑定  
  
1. 创建 <xref:System.ServiceModel.BasicHttpBinding> 类的一个实例，并将其安全模式设置为消息级。  
  
     [!code-csharp[C_HowTo_ChangeStandardBinding#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_changestandardbinding/cs/program.cs#1)]
     [!code-vb[C_HowTo_ChangeStandardBinding#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_changestandardbinding/vb/program.vb#1)]  
  
2. 从该绑定创建一个自定义绑定，并从该自定义绑定的一个属性创建 <xref:System.ServiceModel.Channels.BindingElementCollection> 类。  
  
     [!code-csharp[C_HowTo_ChangeStandardBinding#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_changestandardbinding/cs/program.cs#2)]
     [!code-vb[C_HowTo_ChangeStandardBinding#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_changestandardbinding/vb/program.vb#2)]  
  
3. 循环通过 <xref:System.ServiceModel.Channels.BindingElementCollection> 类，并在查找 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> 类时，将其 <xref:System.ServiceModel.Channels.HttpTransportBindingElement.KeepAliveEnabled%2A> 属性设置为 `false`。  
  
     [!code-csharp[C_HowTo_ChangeStandardBinding#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_changestandardbinding/cs/program.cs#3)]
     [!code-vb[C_HowTo_ChangeStandardBinding#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_changestandardbinding/vb/program.vb#3)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.BasicHttpBinding>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [自定义绑定](custom-bindings.md)
