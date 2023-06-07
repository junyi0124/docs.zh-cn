---
title: 如何：创建双工联合绑定
ms.date: 03/30/2017
ms.assetid: 4331d2bc-5455-492a-9189-634a82597726
ms.openlocfilehash: e93651ce9fe9dae55c299fcb061da6bdc4b6bc5e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598965"
---
# <a name="how-to-create-a-duplex-federated-binding"></a>如何：创建双工联合绑定

<xref:System.ServiceModel.WSFederationHttpBinding> 仅支持数据报和请求/答复消息交换协定。 若要使用双工消息交换协定，必须创建自定义绑定。 下面的过程演示如何使用 HTTP 和 TCP 传输协议的消息模式安全设置，以及使用 TCP 传输协议的混合模式安全设置，在配置中执行此操作。 本主题的结尾是演示所有 3 个绑定的示例代码。

还可以在代码中创建绑定。 有关要创建的绑定元素堆栈的说明，请参阅[如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)。

## <a name="to-create-a-duplex-federated-custom-binding-with-http"></a>使用 HTTP 创建双工联合自定义绑定

1. 在 [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 配置文件的节点中，创建一个 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 元素。

2. 在 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 元素内，创建一个 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) `name` 属性设置为的元素 `FederationDuplexHttpMessageSecurityBinding` 。

3. 在 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 元素内，创建一个 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) `authenticationMode` 属性设置为的元素 `SecureConversation` 。

4. 在 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素内创建一个 [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 元素，该元素的 `authenticationMode` 属性设置为 `IssuedTokenForCertificate` 或 `IssuedTokenForSslNegotiated` 。

5. 在 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素后，创建一个空 [\<compositeDuplex>](../../configure-apps/file-schema/wcf/compositeduplex.md) 元素。

6. 在 [\<compositeDuplex>](../../configure-apps/file-schema/wcf/compositeduplex.md) 元素后，创建一个空 [\<oneWay>](../../configure-apps/file-schema/wcf/oneway.md) 元素。

7. 在 [\<oneWay>](../../configure-apps/file-schema/wcf/oneway.md) 元素后，创建一个空 [\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md) 元素。

## <a name="to-create-a-duplex-federated-custom-binding-with-tcp-message-security-mode"></a>使用 TCP 消息安全模式创建双工联合自定义绑定

1. 在 [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 配置文件的节点中，创建一个 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 元素。

2. 在 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 元素内，创建一个 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) `name` 属性设置为的元素 `FederationDuplexTcpMessageSecurityBinding` 。

3. 在 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 元素内，创建一个 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) `authenticationMode` 属性设置为的元素 `SecureConversation` 。

4. 在 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素内创建一个 [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 元素，该元素的 `authenticationMode` 属性设置为 `IssuedTokenForCertificate` 或 `IssuedTokenForSslNegotiated` 。

5. 在 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素后，创建一个空 [\<tcpTransport>](../../configure-apps/file-schema/wcf/tcptransport.md) 元素。

## <a name="to-create-a-duplex-federated-custom-binding-with-tcp-mixed-security-mode"></a>使用 TCP 混合安全模式创建双工联合自定义绑定

1. 在 [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 配置文件的节点中，创建一个 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 元素。

2. 在 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 元素内，创建一个 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) `name` 属性设置为的元素 `FederationDuplexTcpTransportSecurityWithMessageCredentialBinding` 。

3. 在 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 元素内，创建一个 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) `authenticationMode` 属性设置为的元素 `SecureConversation` 。

4. 在 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素内创建一个 [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 元素，该元素的 `authenticationMode` 属性设置为 `IssuedTokenForCertificate` 或 `IssuedTokenForSslNegotiated` 。

5. 在 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素后，创建一个空 [\<sslStreamSecurity>](../../configure-apps/file-schema/wcf/sslstreamsecurity.md) 元素。

6. 在 [\<sslStreamSecurity>](../../configure-apps/file-schema/wcf/sslstreamsecurity.md) 元素后，创建一个空 [\<tcpTransport>](../../configure-apps/file-schema/wcf/tcptransport.md) 元素。

## <a name="code-sample"></a>代码示例

### <a name="sample-with-3-bindings"></a>包含 3 个绑定的示例

1. 将以下代码插入到配置文件中。

## <a name="example"></a>示例

```xml
<bindings>
   <customBinding>
      <binding name="FederationDuplexHttpMessageSecurityBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenForSslNegotiated" />
          </security>
          <compositeDuplex />
          <oneWay />
          <httpTransport />
       </binding>
<!-- duplex over https is not supported -->
       <binding name="FederationDuplexTcpMessageSecurityBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenForSslNegotiated" />
          </security>
          <tcpTransport />
       </binding>
       <binding name="FederationDuplexTcpTransportSecurityWithMessageCredentialsBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenOverTransport" />
          </security>
<!-- requireClientCertificate = true or <windowsStreamSecurity /> can be used, but does not make sense for most scenarios -->
          <sslStreamSecurity />
          <tcpTransport />
       </binding>
    </customBinding>
</bindings>
```
