---
title: <message> 的 <netHttpBinding>
ms.date: 03/30/2017
ms.assetid: 9def5a35-475d-40d6-b716-ccdbd93863c7
ms.openlocfilehash: b37d6b1bd8f5372c230acbc2ada299415b2e40b3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204848"
---
# <a name="message-of-nethttpbinding"></a>\<message> 的 \<netHttpBinding>

定义的消息级安全性设置 [\<netHttpBinding>](nethttpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netHttpBinding>**](nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<message>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
         clientCredentialType="UserName/Certificate" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|algorithmSuite|设置消息加密和密钥包装算法。 此属性类型为 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>，用于指定算法和密钥大小。 这些算法与“安全策略语言”(WS-SecurityPolicy) 规范中指定的算法一致。<br /><br /> 默认值是 `Basic256`。|  
|clientCredentialType|指定要在使用基于消息的安全性执行客户端身份验证时使用的凭据类型。 默认为 `UserName`。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|UserName|-要求使用用户名凭据对客户端进行身份验证。 需要使用 <> 元素来指定此凭据 `clientCredentials` 。<br />-WCF 不支持发送密码摘要，也不支持使用密码派生密钥并使用此类密钥来实现消息安全性。 因此，WCF 强制在使用用户名凭据时确保传输安全。 对于 `basicHttpBinding`，这要求设置一个 SSL 通道。|  
|证书|要求使用证书向服务器对客户端进行身份验证。 在这种情况下，需要使用 <`clientCredentials`> 和 <> 来指定客户端凭据 `clientCertificate` 。 此外，在使用消息安全模式时，需要向客户端提供服务证书。 在这种情况下，需要使用 <xref:System.ServiceModel.Description.ClientCredentials> 类或行为元素来指定服务凭据 `ClientCredentials` ，并使用 serviceCredentials 的元素指定服务证书 \<serviceCertificate> 。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|<`security` <的> 元素 `netHttpBinding`>|定义 <> 元素的安全功能 `netHttpBinding` 。|  
  
## <a name="example"></a>示例  

 此示例演示如何实现使用 basicHttpBinding 和消息安全性的应用程序。 在下面的服务配置示例中，终结点定义将指定 basicHttpBinding 并引用名为 `Binding1` 的绑定配置。 服务用于向客户端验证自己身份的证书是在配置文件的 `behaviors` 节中 `serviceCredentials` 元素的下面设置的。 应用于证书（客户端使用该证书向服务验证自己的身份）的验证模式也是在 `behaviors` 节中 `clientCertificate` 元素的下面设置的。  
  
 在客户端配置文件中指定同样的绑定和安全详细信息。  
  
```xml  
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <host>
        <baseAddresses>
          <add baseAddress="http://localhost:8000/ServiceModelSamples/service" />
        </baseAddresses>
      </host>
      <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service  -->
      <endpoint address=""
                binding="basicHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
      <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
      <endpoint address="mex"
                binding="mexHttpBinding"
                contract="IMetadataExchange" />
    </service>
  </services>
  <bindings>
    <basicHttpBinding>
      <!-- This configuration defines the SecurityMode as Message and
           the clientCredentialType as Certificate. -->
      <binding name="Binding1" >
        <security mode = "Message">
          <message clientCredentialType="Certificate" />
        </security>
      </binding>
    </basicHttpBinding>
  </bindings>
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceMetadata httpGetEnabled="True" />
        <serviceDebug includeExceptionDetailInFaults="False" />
        <!-- The serviceCredentials behavior allows one to define a service certificate.
             A service certificate is used by a client to authenticate the service and provide message protection.
             This configuration references the "localhost" certificate installed during the setup instructions. -->
        <serviceCredentials>
          <serviceCertificate findValue="localhost"
                              storeLocation="LocalMachine"
                              storeName="My"
                              x509FindType="FindBySubjectName" />
          <clientCertificate>
            <!-- Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
                 is in the user's Trusted People store, then it will be trusted without performing a
                 validation of the certificate's issuer chain. This setting is used here for convenience so that the
                 sample can be run without having to have certificates issued by a certification authority (CA).
                 This setting is less secure than the default, ChainTrust. The security implications of this
                 setting should be carefully considered before using PeerOrChainTrust in production code. -->
            <authentication certificateValidationMode="PeerOrChainTrust" />
          </clientCertificate>
        </serviceCredentials>
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```  
  
## <a name="see-also"></a>请参阅

- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
