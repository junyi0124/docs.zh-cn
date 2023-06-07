---
title: 如何：设置安全模式
description: 了解如何在大多数预定义的绑定上设置三个常见的 WCF 安全模式： Transport、Message 和 TransportWithMessageCredential。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Mode property
- WCF, security mode
- WCF, security
ms.assetid: 6e01dd9f-b5dd-4474-b24c-06e124de4ff7
ms.openlocfilehash: 2f834e1930b7676592f6cbc29a577424d75ebc01
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244538"
---
# <a name="how-to-set-the-security-mode"></a>如何：设置安全模式

Windows Communication Foundation （WCF）安全在大多数预定义的绑定上找到三个常见安全模式：传输、消息和 "带有消息凭据的传输"。 另外，还有两种特定于两个绑定的模式：<xref:System.ServiceModel.BasicHttpBinding> 上的“transport-credential only”模式和 <xref:System.ServiceModel.NetMsmqBinding> 上的“Both”模式。 不过，本主题主要讨论三种常见安全模式：<xref:System.ServiceModel.SecurityMode.Transport>、<xref:System.ServiceModel.SecurityMode.Message> 和 <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential>。

请注意，并非所有预定义绑定都支持所有这些模式。 本主题使用 <xref:System.ServiceModel.WSHttpBinding> 和 <xref:System.ServiceModel.NetTcpBinding> 类来设置模式，并演示如何以编程方式来设置安全模式，如何通过配置来设置安全模式。

有关详细信息，请参阅 WCF 安全性、[安全概述](./feature-details/security-overview.md)、[保护服务](securing-services.md)和[保护服务和客户端](./feature-details/securing-services-and-clients.md)。 有关传输模式和消息的详细信息，请参阅[传输安全性](./feature-details/transport-security.md)和[消息安全性](./feature-details/message-security-in-wcf.md)。

## <a name="to-set-the-security-mode-in-code"></a>在代码中设置安全模式

1. 创建要使用的绑定类的一个实例。 有关预定义绑定的列表，请参阅[系统提供的绑定](system-provided-bindings.md)。 此示例创建 <xref:System.ServiceModel.WSHttpBinding> 类的一个实例。

2. 设置 `Mode` 属性所返回的对象的 `Security` 属性。

     [!code-csharp[c_SettingSecurityMode#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#1)]
     [!code-vb[c_SettingSecurityMode#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#1)]

     或者，设置为消息模式，如下面的代码所示。

     [!code-csharp[c_SettingSecurityMode#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#2)]
     [!code-vb[c_SettingSecurityMode#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#2)]

     或者，设置为使用消息凭据的传输模式，如下面的代码所示。

     [!code-csharp[c_SettingSecurityMode#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#3)]
     [!code-vb[c_SettingSecurityMode#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#3)]

3. 此外，还可以在绑定的构造函数中设置模式，如下面的代码所示。

     [!code-csharp[c_SettingSecurityMode#4](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#4)]
     [!code-vb[c_SettingSecurityMode#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#4)]

## <a name="setting-the-clientcredentialtype-property"></a>设置 ClientCredentialType 属性

将模式设置为三个值之一，就确定了 `ClientCredentialType` 属性的设置方式。 例如，使用 <xref:System.ServiceModel.WSHttpBinding> 类将模式设置为 `Transport`，意味着必须将 <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A> 类的 <xref:System.ServiceModel.HttpTransportSecurity> 属性设置为相应的值。

### <a name="to-set-the-clientcredentialtype-property-for-transport-mode"></a>针对传输模式设置 ClientCredentialType 属性

1. 创建绑定的一个实例。

2. 将 `Mode` 属性设置为 `Transport`。

3. 将 `ClientCredential` 属性设置为适当的值。 下面的代码将该属性设置为 `Windows`。

     [!code-csharp[c_SettingSecurityMode#5](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#5)]
     [!code-vb[c_SettingSecurityMode#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#5)]

### <a name="to-set-the-clientcredentialtype-property-for-message-mode"></a>针对消息模式设置 ClientCredentialType 属性

1. 创建绑定的一个实例。

2. 将 `Mode` 属性设置为 `Message`。

3. 将 `ClientCredential` 属性设置为适当的值。 下面的代码将该属性设置为 `Certificate`。

     [!code-csharp[c_SettingSecurityMode#6](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#6)]
     [!code-vb[c_SettingSecurityMode#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#6)]

### <a name="to-set-the-mode-and-clientcredentialtype-property-in-configuration"></a>在配置中设置 Mode 和 ClientCredentialType 属性

1. 将相应的绑定元素添加到 [\<bindings>](../configure-apps/file-schema/wcf/bindings.md) 配置文件的元素中。 下面的示例添加一个 [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) 元素。

2. 添加 `<binding>` 元素，并将其 `name` 属性设置为合适的值。

3. 添加一个 `<security>` 元素，并将 `mode` 属性设置为 `Message`、`Transport` 或 `TransportWithMessageCredential`。

4. 如果模式设置为 `Transport`，则添加一个 `<transport>` 元素，并将 `clientCredential` 属性设置为适当的值。

     下面的示例将模式设置为 "`Transport"`，并将 `clientCredentialType` 元素的 `<transport>` 属性设置为 "`Windows"`。

    ```xml
    <wsHttpBinding>
    <binding name="TransportSecurity">
        <security mode="Transport" >
           <transport clientCredentialType = "Windows" />
        </security>
    </binding>
    </wsHttpBinding >
    ```

     或者，将 `security mode` 设置为 "`Message"`，后接一个 `<"message">` 元素。 本示例将 `clientCredentialType` 设置为 "`Certificate"`。

    ```xml
    <wsHttpBinding>
    <binding name="MessageSecurity">
        <security mode="Message" >
           <message clientCredentialType = "Certificate" />
        </security>
    </binding>
    </wsHttpBinding >
    ```

     使用 <xref:System.ServiceModel.BasicHttpSecurityMode.TransportWithMessageCredential> 值是一种特殊情况，下面将进行说明。

### <a name="using-transportwithmessagecredential"></a>使用 TransportWithMessageCredential

在将安全模式设置为 `TransportWithMessageCredential` 时，传输会确定实际提供传输级安全的机制。 例如，HTTP 协议使用基于 HTTP 的安全套接字层 (SSL)（SSL over HTPP，或 HTTPS）。 因此，对任何传输安全对象（例如 `ClientCredentialType`）的 <xref:System.ServiceModel.HttpTransportSecurity> 属性进行的设置都将被忽略。  换言之，只能设置消息安全对象（对于 `ClientCredentialType` 绑定，则是 `WSHttpBinding` 对象）的 <xref:System.ServiceModel.NonDualMessageSecurityOverHttp>。

有关详细信息，请参阅[如何：使用传输安全和消息凭据](./feature-details/how-to-use-transport-security-and-message-credentials.md)。

## <a name="see-also"></a>请参阅

- [如何：使用 SSL 证书配置端口](./feature-details/how-to-configure-a-port-with-an-ssl-certificate.md)
- [如何：使用传输安全和消息凭据](./feature-details/how-to-use-transport-security-and-message-credentials.md)
- [传输安全](./feature-details/transport-security.md)
- [消息安全](./feature-details/message-security-in-wcf.md)
- [安全性概述](./feature-details/security-overview.md)
- [系统提供的绑定](system-provided-bindings.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-wshttpbinding.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-basichttpbinding.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-nettcpbinding.md)
