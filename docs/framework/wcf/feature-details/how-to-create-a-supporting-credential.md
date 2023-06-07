---
title: 如何：创建支持凭据
ms.date: 03/30/2017
ms.assetid: d0952919-8bb4-4978-926c-9cc108f89806
ms.openlocfilehash: 1e11da11de68b1d3e24115387ec61ad22ec031b1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286300"
---
# <a name="how-to-create-a-supporting-credential"></a>如何：创建支持凭据

自定义安全方案可能要求提供多个凭据。 例如，某个服务可能要求客户端不仅提供用户名和密码，还要提供能够证明客户端用户已满 18 岁的凭据。 第二个凭据是 *支持凭据*。 本主题说明如何在 Windows Communication Foundation (WCF) 客户端中实现此类凭据。  
  
> [!NOTE]
> 支持凭据的规范是 WS-SecurityPolicy 规范的一部分。 有关详细信息，请参阅 [Web Services 安全性规范](/previous-versions/dotnet/articles/ms951273(v=msdn.10))。  
  
## <a name="supporting-tokens"></a>支持令牌  

 简而言之，使用消息安全性时，始终使用 *主凭据* 来保护消息 (例如，x.509 证书或 Kerberos 票证) 。  
  
 根据规范的定义，安全绑定使用 *令牌* 来保护消息交换。 *令牌* 是安全凭据的表示形式。  
  
 安全绑定使用在安全绑定策略中标识的主令牌来创建签名。 此签名称为 *消息签名*。  
  
 还可以指定其他令牌，以扩充与消息签名相关联的令牌所提供的声明。  
  
## <a name="endorsing-signing-and-encrypting"></a>认可、签名和加密  

 支持凭据会导致在消息内传输 *支持令牌* 。 WS-SecurityPolicy 规范定义了四种将支持令牌附加到消息的方法，如下表所述。  
  
|目的|描述|  
|-------------|-----------------|  
|有符号|支持令牌包含在安全标头中，并由消息签名进行签名。|  
|认可|*认可令牌* 对消息签名进行签名。|  
|签名和认可|签名的认可令牌对从消息签名生成的整个 `ds:Signature` 元素进行签名，并由该消息签名对自己进行签名；也就是说，两种令牌（用于消息签名的令牌和已签名的认可令牌）互相进行签名。|  
|签名和加密|签名的加密支持令牌是在 `wsse:SecurityHeader` 中出现时还进行加密的签名支持令牌。|  
  
## <a name="programming-supporting-credentials"></a>对支持凭据进行编程  

 若要创建使用支持令牌的服务，必须创建 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 。  (有关详细信息，请参阅 [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)。 )   
  
 创建自定义绑定的第一步是创建一个安全绑定元素，该元素可以是以下三种类型之一：  
  
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>  
  
 所有类都继承自 <xref:System.ServiceModel.Channels.SecurityBindingElement>，它包含四个相关属性：  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.EndpointSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OperationSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OptionalEndpointSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OptionalOperationSupportingTokenParameters%2A>  
  
#### <a name="scopes"></a>作用域  

 支持凭据存在两个范围：  
  
- 支持 *令牌的终结* 点支持终结点的所有操作。 即，每当调用任何终结点操作，都可以使用该支持令牌表示的凭据。  
  
- 支持 *令牌的操作* 仅支持特定的终结点操作。  
  
 正如属性名所指示的那样，支持凭据可能是必需的，也可能是可选的。 即，如果支持凭据存在，则使用支持凭据（尽管不是必需的），但如果支持凭据不存在，身份验证也不会失败。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-create-a-custom-binding-that-includes-supporting-credentials"></a>创建一个包含支持凭据的自定义绑定  
  
1. 创建一个安全绑定元素。 下面的示例创建一个 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>，该元素具有 `UserNameForCertificate` 身份验证模式。 使用 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForCertificateBindingElement%2A> 方法。  
  
2. 将支持参数添加到由适当的属性（`Endorsing`、`Signed`、`SignedEncrypted` 或 `SignedEndorsed`）返回的类型集合中。 <xref:System.ServiceModel.Security.Tokens> 命名空间中的类型包括常用类型，如 <xref:System.ServiceModel.Security.Tokens.X509SecurityTokenParameters>。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例创建 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 的一个实例，并将 <xref:System.ServiceModel.Security.Tokens.KerberosSecurityTokenParameters> 类的一个实例添加到 Endorsing 属性返回的集合中。  
  
### <a name="code"></a>代码  

 [!code-csharp[c_SupportingCredential#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_supportingcredential/cs/source.cs#1)]  
  
## <a name="see-also"></a>请参阅

- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
