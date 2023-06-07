---
title: <issuerTokenResolver>
ms.date: 03/30/2017
ms.assetid: f74392f6-3f5b-4880-bd8a-3a9130d31e65
author: BrucePerlerMS
ms.openlocfilehash: 946ae8601e1e4563becd0b346b6c792724405a45
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165034"
---
# \<issuerTokenResolver>

注册由标记处理程序集合中的处理程序使用的颁发者令牌解析程序。 颁发者令牌解析器用于解析传入令牌和消息的签名令牌。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlerConfiguration>**](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<issuerTokenResolver>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <issuerTokenResolver type=xs:string>  
        </issuerTokenResolver>  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|type|指定颁发者令牌解析程序的类型。 必须是 <xref:System.IdentityModel.Tokens.IssuerTokenResolver> 类或派生自类的类型 <xref:System.IdentityModel.Tokens.IssuerTokenResolver> 。 必需。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|为安全标记处理程序的集合提供配置。|  
  
## <a name="remarks"></a>备注  

 颁发者令牌解析器用于解析传入令牌和消息的签名令牌。 它用于检索用于检查签名的加密材料。 您必须指定 `type` 属性。 指定的类型可以是 <xref:System.IdentityModel.Tokens.IssuerTokenResolver> 或从类派生的自定义类型 <xref:System.IdentityModel.Tokens.IssuerTokenResolver> 。  
  
 某些标记处理程序允许您在配置中指定颁发者令牌解析器设置。 各个标记处理程序上的设置将替代在安全令牌处理程序集合中指定的设置。  
  
> [!NOTE]
> 将 `<issuerTokenResolver>` 元素指定为元素的子元素 [\<identityConfiguration>](identityconfiguration.md) 已不推荐使用，但仍支持向后兼容。 元素上的设置将 `<securityTokenHandlerConfiguration>` 替代元素上的设置 `<identityConfiguration>` 。  
  
## <a name="example"></a>示例  

 下面的 XML 显示基于从派生的自定义类的颁发者令牌解析程序的配置 <xref:System.IdentityModel.Tokens.IssuerTokenResolver> 。 令牌解析程序将维护从 `<AddAudienceKeyPair>`) 为类定义 (自定义配置元素初始化的受众密钥对字典。 类将重写 <xref:System.IdentityModel.Selectors.SecurityTokenResolver.LoadCustomConfiguration%2A> 方法来处理此元素。 下面的示例显示了替代：但对于简洁起见，它调用的方法不会显示。 有关完整的示例，请参阅 `CustomToken` 示例。  
  
```xml  
<issuerTokenResolver type="SimpleWebToken.CustomIssuerTokenResolver, SimpleWebToken">  
  <AddAudienceKeyPair  symmetricKey="wAVkldQiFypTQ+kdNdGWCYCHRcee8XmXxOvgmak8vSY=" audience="http://localhost:19851/" />  
</issuerTokenResolver>  
```  
  
## <a name="example"></a>示例
  
```csharp
public override void LoadCustomConfiguration(System.Xml.XmlNodeList nodelist)  
{  
    foreach (XmlNode node in nodelist)  
    {  
        XmlDictionaryReader rdr = XmlDictionaryReader.CreateDictionaryReader(new XmlTextReader(new StringReader(node.OuterXml)));  
        rdr.MoveToContent();  
  
        string symmetricKey = rdr.GetAttribute("symmetricKey");  
        string audience = rdr.GetAttribute("audience");  
  
        this.AddAudienceKeyPair(audience, symmetricKey);  
    }  
}  
```
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Tokens.IssuerTokenResolver>
