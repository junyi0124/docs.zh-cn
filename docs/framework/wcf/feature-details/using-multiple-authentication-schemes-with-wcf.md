---
title: 对 WCF 使用多个身份验证方案
ms.date: 03/30/2017
ms.assetid: f32a56a0-e2b2-46bf-a302-29e1275917f9
ms.openlocfilehash: 3aae9bff4300af97f7b179d9d8115340a26e715a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289420"
---
# <a name="using-multiple-authentication-schemes-with-wcf"></a>对 WCF 使用多个身份验证方案

WCF 现在允许您对单个终结点上指定多个身份验证方案。 此外，Web 承载的服务可以直接从 IIS 继承其身份验证设置。 自承载服务可以指定可使用的身份验证方案。 有关在 IIS 中设置身份验证设置的详细信息，请参阅 [Iis 身份验证](https://go.microsoft.com/fwlink/?LinkId=232458)  
  
## <a name="iis-hosted-services"></a>承载于 IIS 中的服务  

 对于承载于 IIS 中的服务，设置您希望在 IIS 中使用的身份验证方案。 然后，在你的服务的 web.config 文件中，在绑定配置中将 clientCredential 类型指定为 "InheritedFromHost"，如下面的 XML 代码段中所示：  
  
```xml  
<bindings>  
      <basicHttpBinding>  
        <binding name="secureBinding">  
          <security mode="Transport">  
            <transport clientCredentialType="InheritedFromHost" />  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
```  
  
 你可以使用 ServiceAuthenticationBehavior 或元素指定你只希望将身份验证方案的一个子集用于服务 \<serviceAuthenticationManager> 。 在代码中对此进行配置时，使用 ServiceAuthenticationBehavior，如下面的 XML 代码段中所示。  
  
```csharp  
// ...  
ServiceAuthenticationBehavior sab = null;  
sab = serviceHost.Description.Behaviors.Find<ServiceAuthenticationBehavior>();  
  
if (sab == null)  
{  
    sab = new ServiceAuthenticationBehavior();  
    sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
    serviceHost.Description.Behaviors.Add(sab);  
}  
else  
{  
     sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
}  
// ...  
```  
  
 在配置文件中配置此配置时，请使用 \<serviceAuthenticationManager> 元素，如下面的 XML 代码段中所示。  
  
```xml  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="limitedAuthBehavior">  
          <serviceAuthenticationManager authenticationSchemes="Negotiate, Digest, Basic"/>  
          <!-- ... -->  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 这将确保只有一部分此处列出的身份验证方案将被考虑应用于服务终结点，具体是哪些身份验证方案取决于 IIS 中所选的内容。 这意味着，开发人员可以通过从 serviceAuthenticationManager 列表中省略基本身份验证而从列表中排除它，甚至即使在 IIS 中启用基本身份验证，它也将不会在服务终结点上应用  
  
## <a name="self-hosted-services"></a>自承载服务  

 自承载服务在配置上稍有不同，因为没有要从其继承设置的 IIS。 在这里，你将使用 \<serviceAuthenticationManager> 元素或 ServiceAuthenticationBehavior 指定将继承的身份验证设置。 在代码中如下所示：  
  
```csharp  
// ...  
ServiceAuthenticationBehavior sab = null;  
sab = serviceHost.Description.Behaviors.Find<ServiceAuthenticationBehavior>();  
  
if (sab == null)  
{  
    sab = new ServiceAuthenticationBehavior();  
    sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
    serviceHost.Description.Behaviors.Add(sab);  
}  
else  
{  
     sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
}  
// ...  
```  
  
 在配置中如下所示：  
  
```xml  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="limitedAuthBehavior">  
          <serviceAuthenticationManager authenticationSchemes="Negotiate, Digest, Basic"/>  
          <!-- ... -->  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 然后，你可以在绑定设置中指定 InheritFromHost，如下面的 XML 代码段中所示。  
  
```xml  
<bindings>  
      <basicHttpBinding>  
        <binding name="secureBinding">  
          <security mode="Transport">  
            <transport clientCredentialType="InheritedFromHost" />  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
```  
  
 或者，你可以通过在 HTTP 传输绑定元素上设置身份验证架构，在自定义绑定中指定身份验证架构，如下面的配置代码段所示。  
  
```xml  
<binding name="multipleBinding">  
      <textMessageEncoding/>  
      <httpTransport authenticationScheme="Negotiate, Ntlm, Digest, Basic" />  
    </binding>  
```  
  
## <a name="see-also"></a>另请参阅

- [绑定与安全](bindings-and-security.md)
- [终结点：地址、绑定和协定](endpoints-addresses-bindings-and-contracts.md)
- [配置系统提供的绑定](configuring-system-provided-bindings.md)
- [使用自定义绑定的安全功能](security-capabilities-with-custom-bindings.md)
- [绑定](bindings.md)
- [自定义绑定](../extending/custom-bindings.md)
