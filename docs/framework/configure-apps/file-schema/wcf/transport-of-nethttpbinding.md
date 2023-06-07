---
title: <transport> 的 <netHttpBinding>
ms.date: 03/30/2017
ms.assetid: 3b180006-1661-43bf-a699-96fd3da469af
ms.openlocfilehash: 996b3655b0698595256c9a7197f705d46e6e9fcf
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169812"
---
# <a name="transport-of-nethttpbinding"></a>\<transport> 的 \<netHttpBinding>

为 HTTP 传输定义控制身份验证参数的属性。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netHttpBinding>**](nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netHttpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential|TransportCredentialOnly">
      <transport clientCredentialType="None|Basic|Digest|Ntlm|Windows"
                 proxyCredentialType="None|Basic|Digest|Ntlm|Windows"
                 realm="string">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</netHttpBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|clientCredentialType|-指定执行使用 HTTP 身份验证的客户端身份验证时要使用的凭据类型。  默认为 `None`。 此属性的类型为 <xref:System.ServiceModel.HttpClientCredentialType>。|  
|proxyCredentialType|-指定使用代理通过 HTTP 在域中执行客户端身份验证时使用的凭据类型。 只有当父 `mode` 元素的 `security` 属性为 `Transport` 或 `TransportCredentialsOnly` 时，此属性才适用。 此属性的类型为 <xref:System.ServiceModel.HttpProxyCredentialType>。|  
|realm|一个字符串，指定摘要式或基本身份验证的 HTTP 身份验证方案所使用的领域。 默认值为空字符串。|  
|policyEnforcement|此枚举指定应何时强制实施 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>。<br /><br /> 1. 从不–不会强制实施策略 (禁用扩展保护) 。<br />WhenSupported-仅当客户端支持扩展保护时才强制实施策略。<br />3. always –始终强制实施策略。 不支持扩展保护的客户端将无法进行身份验证。|  
|protectionScenario|此枚举指定此策略强制实施的保护方案。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|在传输过程中不能保证消息的安全。|  
|基本|指定基本身份验证。|  
|摘要|指定摘要式身份验证。|  
|Ntlm|指定 NTLM 身份验证（如果可能且 Windows 身份验证失败）。|  
|Windows|指定 Windows 集成身份验证。|  
  
## <a name="proxycredentialtype-attribute"></a>proxyCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|-传输过程中消息不受保护。|  
|基本|指定“RFC 2617 – HTTP 身份验证：基本和摘要式身份验证”所定义的基本身份验证。|  
|摘要|指定“RFC 2617 – HTTP 身份验证：基本和摘要式身份验证”所定义的摘要式身份验证。|  
|Ntlm|指定 NTLM 身份验证（如果可能且 Windows 身份验证失败）。|  
|Windows|指定 Windows 集成身份验证。|  
|证书|使用证书执行客户端身份验证。 此选项只在父 `Mode` 元素的 `security` 属性设置为“Transport”时才起作用，如果该属性设置为“TransportCredentialOnly”，则此选项将不起作用。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-nethttpbinding.md)|定义的安全功能 [\<netHttpBinding>](nethttpbinding.md) 。|  
  
## <a name="example"></a>示例  

 下面的示例演示如何对基本绑定使用 SSL 传输安全。 默认情况下，基本绑定支持 HTTP 通信。  
  
```xml  
<system.serviceModel>
  <services>
    <service type="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <endpoint address=""
                binding="netHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>
  <bindings>
    <netHttpBinding>
      <!-- Configure basicHttpBinding with Transport security -->
      <!-- mode and clientCredentialType set to None. -->
      <binding name="Binding1">
        <security mode="Transport">
          <transport clientCredentialType="None"
                     proxyCredentialType="None">
            <extendedProtectionPolicy policyEnforcement="WhenSupported"
                                      protectionScenario="TransportSelected">
              <customServiceNames>
              </customServiceNames>
            </extendedProtectionPolicy>
          </transport>
        </security>
      </binding>
    </netHttpBinding>
  </bindings>
</system.serviceModel>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.BasicHttpSecurityMode.Transport>
- <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement>
- <xref:System.ServiceModel.HttpTransportSecurity>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
