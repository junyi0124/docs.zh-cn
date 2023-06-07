---
title: 如何：配置 WCF 服务以与 WSE 3.0 客户端进行互操作
ms.date: 03/30/2017
ms.assetid: 0f38c4a0-49a6-437c-bdde-ad1d138d3c4a
ms.openlocfilehash: 600b9c28d92f9e2b6e4d586b052cc5762d591521
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599056"
---
# <a name="how-to-configure-wcf-services-to-interoperate-with-wse-30-clients"></a>如何：配置 WCF 服务以与 WSE 3.0 客户端进行互操作

当 WCF 服务配置为使用8月2004版的 WS-ADDRESSING 规范时，Windows Communication Foundation （WCF）服务与用于 Microsoft .NET （WSE）客户端的 Web 服务增强3.0 线路级别兼容。

### <a name="to-enable-a-wcf-service-to-interoperate-with-wse-30-clients"></a>使 WCF 服务与 WSE 3.0 客户端进行互操作

1. 定义 WCF 服务的自定义绑定。

    若要指定将 WS-Addressing 规范的 2004 年 8 月版用于消息编码，必须创建自定义绑定。

    1. 将子项添加 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 到 [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 服务配置文件的。

    2. 通过将添加 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 到 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 并设置属性，指定绑定的名称 `name` 。

    3. 通过将子添加到，指定身份验证模式和用于保护与 WSE 3.0 兼容的消息的 WS 安全规范的版本 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 。

        若要设置身份验证模式，请设置 `authenticationMode` 的属性 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 。 身份验证模式大致等效于 WSE 3.0 中的关守安全断言。 下表将 WCF 中的身份验证模式映射到 WSE 3.0 中的交钥匙安全声明。

        |WCF 身份验证模式|WSE 3.0 关守安全断言|
        |-----------------------------|----------------------------------------|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.AnonymousForCertificate>|`anonymousForCertificateSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.Kerberos>|`kerberosSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate>|`mutualCertificate10Security`*|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate>|`mutualCertificate11Security`*|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.UserNameOverTransport>|`usernameOverTransportSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.UserNameForCertificate>|`usernameForCertificateSecurity`|

        \*`mutualCertificate10Security`和 `mutualCertificate11Security` 交钥匙安全声明的主要区别之一是 WSE 用于保护 SOAP 消息的 WS 安全规范的版本。 `mutualCertificate10Security` 使用 WS-Security 1.0，而 WS-Security 1.1 用于 `mutualCertificate11Security`。 对于 WCF，会在的属性中指定 WS 安全规范的版本 `messageSecurityVersion` [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 。

        若要设置用于保护 SOAP 消息的 WS 安全规范的版本，请设置 `messageSecurityVersion` 的属性 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 。 若要与 WSE 3.0 进行互操作，请将 `messageSecurityVersion` 属性的值设置为 <xref:System.ServiceModel.MessageSecurityVersion.WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10%2A>。

    4. 通过添加 [\<textMessageEncoding>](../../configure-apps/file-schema/wcf/textmessageencoding.md) 并将其值设置为，指定 WCF 使用 ws-addressing 规范的8月2004版 `messageVersion` <xref:System.ServiceModel.Channels.MessageVersion.Soap11WSAddressingAugust2004%2A> 。

        > [!NOTE]
        > 在使用 SOAP 1.2 时，请将 `messageVersion` 属性设置为 <xref:System.ServiceModel.Channels.MessageVersion.Soap12WSAddressingAugust2004%2A>。

2. 指定该服务使用自定义绑定。

    1. 将 `binding` 元素的属性设置 [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) 为 `customBinding` 。

    2. 将 `bindingConfiguration` 元素的属性设置 [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) 为 `name` 自定义绑定的的特性中指定的值 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 。

## <a name="example"></a>示例

下面的代码示例指定 `Service.HelloWorldService` 使用自定义绑定与 WSE 3.0 客户端进行互操作。 自定义绑定指定使用 WS-Addressing 的 2004 年 8 月版本和 WS-Security 1.1 规范集对所交换的消息进行编码。 使用 <xref:System.ServiceModel.Configuration.AuthenticationMode.AnonymousForCertificate> 身份验证模式保证消息的安全。

```xml
<configuration>
  <system.serviceModel>
    <services>
      <service
        behaviorConfiguration="ServiceBehavior"
        name="Service.HelloWorldService">
        <endpoint binding="customBinding" address=""
          bindingConfiguration="ServiceBinding"
          contract="Service.IHelloWorld"></endpoint>
      </service>
    </services>

    <bindings>
      <customBinding>
        <binding name="ServiceBinding">
          <security authenticationMode="AnonymousForCertificate"
                  messageProtectionOrder="SignBeforeEncrypt"
                  messageSecurityVersion="WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10"
                  requireDerivedKeys="false">
          </security>
          <textMessageEncoding messageVersion ="Soap11WSAddressingAugust2004"></textMessageEncoding>
          <httpTransport/>
        </binding>
      </customBinding>
    </bindings>
    <behaviors>
      <behavior name="ServiceBehavior" returnUnknownExceptionsAsFaults="true">
        <serviceCredentials>
          <serviceCertificate findValue="CN=WCFQuickstartServer" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectDistinguishedName"/>
        </serviceCredentials>
      </behavior>
    </behaviors>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [如何：自定义系统提供的绑定](../extending/how-to-customize-a-system-provided-binding.md)
