---
title: 如何：为指定的身份验证模式创建 SecurityBindingElement
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a7c7747a-5b8c-463f-8493-7266dac75066
ms.openlocfilehash: 7b71224c74d7e9e766fb17101219dc5718d5d6a6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286430"
---
# <a name="how-to-create-a-securitybindingelement-for-a-specified-authentication-mode"></a>如何：为指定的身份验证模式创建 SecurityBindingElement

Windows Communication Foundation (WCF) 提供多种模式，客户端和服务通过这些模式进行身份验证。 你可以通过在 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类上使用静态方法或通过配置来为这些身份验证模式创建安全绑定元素，如下面的示例所示。  
  
 有关18种身份验证模式的详细信息，请参阅 [SecurityBindingElement Authentication 模式](securitybindingelement-authentication-modes.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例演示用于为各种身份验证模式创建绑定的方法。  
  
> [!NOTE]
> 一旦创建了 <xref:System.ServiceModel.Channels.SecurityBindingElement> 对象的实例，大量属性（例如 <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters.KeyType%2A> 和 <xref:System.ServiceModel.Channels.SecurityBindingElement.MessageSecurityVersion%2A>）就是不可变的。 对此类属性调用 `set` 后，它们不会发生更改。  
  
 [!code-csharp[c_CustomBindingsAuthMode#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombindingsauthmode/cs/source.cs#2)]
 [!code-vb[c_CustomBindingsAuthMode#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombindingsauthmode/vb/source.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [SecurityBindingElement 身份验证模式](securitybindingelement-authentication-modes.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
