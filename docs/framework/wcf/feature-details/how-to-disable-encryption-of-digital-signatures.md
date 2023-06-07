---
title: 如何：禁用数字签名的加密
ms.date: 03/30/2017
ms.assetid: fd174313-ad81-4dca-898a-016ccaff8187
ms.openlocfilehash: 6d9102ab7676a8e751e608b6081da751464e6ee0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96237386"
---
# <a name="how-to-disable-encryption-of-digital-signatures"></a>如何：禁用数字签名的加密

默认情况下，消息已经过签名，并且已对签名进行了数字加密。 这可以通过以下方法进行控制，即使用 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 或 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 的实例创建一个自定义绑定，然后将其中一个类的 `MessageProtectionOrder` 属性设置为 <xref:System.ServiceModel.Security.MessageProtectionOrder> 枚举值。 默认值为 <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature>。 此过程比只进行签名和加密多用了 30% 的时间，签名和加密的时间取决于整个消息的大小（消息越小，对性能的影响越大）。 但是，禁用签名的加密可能会使攻击者猜测消息的内容。 这种情况是可能的，原因是签名元素包含消息中每个签名部分的纯文本的哈希代码。 例如，尽管默认情况下加密了消息正文，但是在加密之前，未加密的签名包含消息正文的哈希代码。 如果签名和加密部分可能值的集比较小，则攻击者也许就可以通过查看哈希值推导内容。 对签名进行加密可以降低此攻击的影响。  
  
 因此，只能在以下情况下禁用签名的加密：当内容的值比较低或可能的内容值集比较大且具有不确定性时；当性能的提升比减少上述攻击的重要性高时。  
  
> [!NOTE]
> 如果加密的消息中没有任何内容，则不会对签名元素进行加密，即使当 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> 属性被设置为 <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature> 时也是如此。 系统提供的绑定同样也会出现此行为；所有由系统提供的绑定都将消息保护顺序设置为 `SignBeforeEncryptAndEncryptSignature`。 但是， (WSDL) WCF 生成的 Web 服务描述语言仍将包含 `<sp:EncryptSignature>` 断言。  
  
### <a name="to-disable-digital-signing"></a>禁用数字签名  
  
1. 创建 <xref:System.ServiceModel.Channels.CustomBinding>。 有关详细信息，请参阅 [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)。  
  
2. 将 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 或 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 添加到绑定集合。  
  
3. 将 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt>，或将 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt>。  
  
## <a name="see-also"></a>另请参阅

- [使用自定义绑定的安全功能](security-capabilities-with-custom-bindings.md)
