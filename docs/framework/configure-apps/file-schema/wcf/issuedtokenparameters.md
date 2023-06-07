---
title: <issuedTokenParameters>
ms.date: 03/30/2017
ms.assetid: 120b3f37-7331-4816-b712-d6aab39655a4
ms.openlocfilehash: c90024a0629f39d160967ca00434e48f682d8933
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157312"
---
# \<issuedTokenParameters>

指定在联合安全方案中颁发的安全令牌的参数。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<issuedTokenParameters>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<issuedTokenParameters defaultMessageSecurityVersion="System.ServiceModel.MessageSecurityVersion"
                       inclusionMode="AlwaysToInitiator/AlwaysToRecipient/Never/Once"
                       keySize="Integer"
                       keyType="AsymmetricKey/BearerKey/SymmetricKey"
                       tokenType="String">
  <additionalRequestParameters />
  <claimTypeRequirements>
    <add claimType="URI"
         isOptional="Boolean" />
  </claimTypeRequirements>
  <issuer address="String"
          binding="" />
  <issuerMetadata address="String" />
</issuedTokenParameters>
```  
  
## <a name="type"></a>类型  

 `Type`  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|defaultMessageSecurityVersion|指定安全规范（WS-Security、WS-Trust、WS-Secure Conversation 和 WS-Security Policy）的版本，绑定必须支持这些安全规范。 此值的类型为 <xref:System.ServiceModel.MessageSecurityVersion>。|  
|inclusionMode|指定令牌包含要求。 此属性的类型为 <xref:System.ServiceModel.Security.Tokens.SecurityTokenInclusionMode>。|  
|keySize|一个指定令牌的密钥大小的整数。 默认值为 256。|  
|keyType|一个指定密钥类型的 <xref:System.IdentityModel.Tokens.SecurityKeyType> 的有效值。 默认为 `SymmetricKey`。|  
|tokenType|一个指定令牌类型的字符串。 默认值为“http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAML”。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<additionalRequestParameters>](additionalrequestparameters-element.md)|一个用于指定附加请求参数的配置元素的集合。|  
|[\<claimTypeRequirements>](claimtyperequirements-element.md)|指定所需声明类型的集合。<br /><br /> 在联合方案中，服务规定有关传入凭据的要求。 例如，传入凭据必须具有某组声明类型。 此集合中的每个元素都指定希望出现在联合凭据中的必选和可选的声明类型。|  
|[\<issuer>](issuer-of-issuedtokenparameters.md)|一个用于指定颁发当前令牌的终结点的配置元素。|  
|[\<issuerMetadata>](issuermetadata-of-issuedtokenparameters.md)|一个用于指定令牌颁发者的元数据的终结点地址的配置元素。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|指定用于启动安全对话服务的默认值。|  
|[\<security>](security-of-custombinding.md)|指定自定义绑定的安全选项。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters>
- <xref:System.ServiceModel.Configuration.IssuedTokenParametersElement>
- <xref:System.ServiceModel.Configuration.SecurityElementBase.IssuedTokenParameters%2A>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [自定义绑定安全性](../../../wcf/samples/custom-binding-security.md)
- [服务标识和身份验证](../../../wcf/feature-details/service-identity-and-authentication.md)
- [联合令牌与颁发的令牌](../../../wcf/feature-details/federation-and-issued-tokens.md)
- [使用自定义绑定的安全功能](../../../wcf/feature-details/security-capabilities-with-custom-bindings.md)
- [联合令牌与颁发的令牌](../../../wcf/feature-details/federation-and-issued-tokens.md)
