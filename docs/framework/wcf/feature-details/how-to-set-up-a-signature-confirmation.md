---
title: 如何：设置签名确认
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- signature confirmation
- WCF, security
ms.assetid: 2424c137-c7c2-4aa9-8d5d-a066e12fefda
ms.openlocfilehash: 9423922753efee7aac32e430f97307c715e43464
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586905"
---
# <a name="how-to-set-up-a-signature-confirmation"></a>如何：设置签名确认

*签名确认*是一种机制，用于消息发起方确保收到的答复是为了响应发送方的原始消息。 WS-Security 1.1 规范中对签名确认进行了定义。 如果终结点支持 WS-Security 1.0，则不能使用签名确认。

下面的过程指定如何使用 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 来启用签名确认。 可以对 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 使用相同的过程。 此过程基于[如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)中的基本步骤。

### <a name="to-enable-signature-confirmation-in-code"></a>在代码中启用签名确认

1. 创建 <xref:System.ServiceModel.Channels.BindingElementCollection> 类的一个实例。

2. 创建类的实例 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 。

3. 将 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.RequireSignatureConfirmation%2A> 设置为 `true`。

4. 将安全元素添加到绑定集合中。

5. 按照[如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)中的说明创建自定义绑定。

### <a name="to-enable-signature-confirmation-in-configuration"></a>在配置中启用签名确认

1. 将 `<customBinding>` 元素添加到配置文件的 `<bindings>` 节。

2. 添加一个 `<binding>` 元素，并将 name 属性设置为适当的值。

3. 添加一个适当的编码元素。 下面的示例添加了一个 `<TextMessageEncoding>` 元素。

4. 添加一个 `<security>` 子元素并将 `requireSignatureConfirmation` 属性设置为 `true`。

5. 可选。 若要在启动过程中启用签名确认，请添加一个 [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 子元素，并将 `requireSignatureConfirmation` 属性设置为 `true` 。

6. 添加一个适当的传输元素。 下面的示例添加一个 [\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md) ：

    ```xml
    <bindings>
      <customBinding>
        <binding name="SignatureConfirmationBinding">
          <security requireSignatureConfirmation="true">
            <secureConversationBootstrap requireSignatureConfirmation="true" />
              </security>
           <textMessageEncoding />
             <httpTransport />
        </binding>
      </customBinding>
    </bindings>
    ```

## <a name="example"></a>示例

下面的代码创建 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 的实例并将 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.RequireSignatureConfirmation%2A> 属性设置为 `true`。 请注意，此示例不使用上一示例中所示的 `<secureConversationBootstrap>` 元素。 此示例演示了使用 Windows（Kerberos 协议）令牌时的签名确认。 在本例中，在来自服务的所有响应中均返回客户端的签名，并且该签名由客户端进行确认。

[!code-csharp[c_SignatureConfirmation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_signatureconfirmation/cs/source.cs#1)]
[!code-vb[c_SignatureConfirmation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_signatureconfirmation/vb/source.vb#1)]

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateBindingElement%2A>
- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
