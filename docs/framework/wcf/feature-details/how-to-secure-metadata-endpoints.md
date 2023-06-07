---
title: 如何：保护元数据终结点
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9f71b6ae-737c-4382-8d89-0a7b1c7e182b
ms.openlocfilehash: c5efd921d3826ef814bf45d6895255981101d992
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592952"
---
# <a name="how-to-secure-metadata-endpoints"></a>如何：保护元数据终结点

服务的元数据中可能包含恶意用户可以利用的关于您的应用程序的敏感信息。 服务使用者可能还要求一种用于获取关于服务的元数据的安全机制。 因此，有时需要使用安全终结点来发布元数据。

通常使用 Windows Communication Foundation （WCF）中定义的标准安全机制来保护元数据终结点，以保护应用程序终结点。 有关详细信息，请参阅[安全性概述](security-overview.md)。

本主题演示创建由安全套接字层 (SSL) 证书保护的终结点（换言之，HTTPS 终结点）的步骤。

### <a name="to-create-a-secure-https-get-metadata-endpoint-in-code"></a>在代码中创建安全的 HTTPS GET 元数据终结点

1. 使用适当的 X.509 证书配置端口。 该证书必须来自受信任的颁发机构，而且还必须有既定的“服务授权”用途。 必须使用 HttpCfg.exe 工具将该证书附加到该端口。 请参阅[如何：使用 SSL 证书配置端口](how-to-configure-a-port-with-an-ssl-certificate.md)。

    > [!IMPORTANT]
    > 该证书或其域名系统 (DNS) 的主题必须与计算机的名称匹配。 这一点非常重要，原因是 HTTPS 机制所执行的前几个步骤之一是检查是否将证书颁发给了在其上调用了该证书的地址的统一资源标识符 (URI)。

2. 创建 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 类的新实例。

3. 将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> 类的 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 属性设置为 `true`。

4. 将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A> 属性设置为适当的 URL。 请注意，如果指定了绝对地址，则 URL 必须以方案开头 `https://` 。 如果指定相对地址，则必须为服务主机提供一个 HTTPS 基址。 如果不设置此属性，则默认地址为 ""，或者直接为服务的 HTTPS 基址。

5. 将该实例添加到 <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> 类的 <xref:System.ServiceModel.Description.ServiceDescription> 属性返回的行为集合中，如下面的代码所示：

    [!code-csharp[c_HowToSecureEndpoint#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosecureendpoint/cs/source.cs#1)]
    [!code-vb[c_HowToSecureEndpoint#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosecureendpoint/vb/source.vb#1)]

### <a name="to-create-a-secure-https-get-metadata-endpoint-in-configuration"></a>在配置中创建安全的 HTTPS GET 元数据终结点

1. 将一个 [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) 元素添加到 [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) 服务的配置文件的元素中。

2. 向 [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) 元素添加元素 [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) 。

3. 向 [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md) 元素添加元素 `<serviceBehaviors>` 。

4. 将 `name` 元素的 `<behavior>` 属性设置为适当的值。 需要 `name` 属性。 下面的示例使用 `mySvcBehavior` 值。

5. 将添加 [\<serviceMetadata>](../../configure-apps/file-schema/wcf/servicemetadata.md) 到 `<behavior>` 元素。

6. 将 `httpsGetEnabled` 元素的 `<serviceMetadata>` 属性设置为 `true`。

7. 将 `httpsGetUrl` 元素的 `<serviceMetadata>` 属性设置为适当的值。 请注意，如果指定了绝对地址，则 URL 必须以方案开头 `https://` 。 如果指定相对地址，则必须为服务主机提供一个 HTTPS 基址。 如果不设置此属性，则默认地址为 ""，或者直接为服务的 HTTPS 基址。

8. 若要将行为与服务一起使用，请将 `behaviorConfiguration` 元素的属性设置 [\<service>](../../configure-apps/file-schema/wcf/service.md) 为行为元素的 name 特性的值。 下面的配置代码演示了一个完整的示例。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
     <system.serviceModel>
      <behaviors>
       <serviceBehaviors>
        <behavior name="mySvcBehavior">
         <serviceMetadata httpsGetEnabled="true"
              httpsGetUrl="https://localhost:8036/calcMetadata" />
        </behavior>
       </serviceBehaviors>
      </behaviors>
     <services>
      <service behaviorConfiguration="mySvcBehavior"
            name="Microsoft.Security.Samples.Calculator">
       <endpoint address="http://localhost:8037/ServiceModelSamples/calculator"
       binding="wsHttpBinding" bindingConfiguration=""
       contract="Microsoft.Security.Samples.ICalculator" />
      </service>
     </services>
    </system.serviceModel>
    </configuration>
    ```

## <a name="example"></a>示例

下面的示例创建 <xref:System.ServiceModel.ServiceHost> 类的一个实例并添加一个终结点。 然后，该代码创建 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 类的一个实例并设置属性以创建一个安全的元数据交换点。

[!code-csharp[c_HowToSecureEndpoint#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosecureendpoint/cs/source.cs#0)]
[!code-vb[c_HowToSecureEndpoint#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosecureendpoint/vb/source.vb#0)]

## <a name="compiling-the-code"></a>编译代码

该代码示例使用下列命名空间：

- <xref:System.ServiceModel?displayProperty=nameWithType>

- <xref:System.ServiceModel.Description?displayProperty=nameWithType>

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A>
- [如何：使用 SSL 证书配置端口](how-to-configure-a-port-with-an-ssl-certificate.md)
- [使用证书](working-with-certificates.md)
- [元数据的安全注意事项](security-considerations-with-metadata.md)
- [保护服务和客户端的安全](securing-services-and-clients.md)
