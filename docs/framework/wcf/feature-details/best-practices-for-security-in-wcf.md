---
title: WCF 中安全性的最佳做法
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- best practices [WCF], security
ms.assetid: 3639de41-1fa7-4875-a1d7-f393e4c8bd69
ms.openlocfilehash: 530574f684a7747a0965e13345feaf4d51e605e9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279345"
---
# <a name="best-practices-for-security-in-wcf"></a>WCF 中安全性的最佳做法

以下各节列出了在使用 Windows Communication Foundation (WCF) 创建安全应用程序时应考虑的最佳做法。 有关安全性的详细信息，请参阅[安全注意事项](security-considerations-in-wcf.md)、[数据的安全注意事项](security-considerations-for-data.md)和[元数据的安全注意事项](security-considerations-with-metadata.md)。  
  
## <a name="identify-services-performing-windows-authentication-with-spns"></a>使用 SPN 标识执行 Windows 身份验证的服务  

 服务可以使用用户主体名称 (UPN) 或服务主体名称 (SPN) 来标识。 使用计算机帐户运行的服务（如网络服务）具有 SPN 标识，该标识对应于正在运行它们的计算机。 使用用户帐户运行的服务具有 UPN 标识，该标识对应于它们正在以其身份运行的用户，但可以使用 `setspn` 工具为该用户帐户分配一个 SPN。 配置服务以便可以通过 SPN 标识它，同时将连接到该服务的客户端配置为使用该 SPN，这可以提高对某些攻击的抵御能力。 此指导信息适用于使用 Kerberos 或 SSPI 协商的绑定。  即使 SSPI 降级为 NTLM，客户端应仍指定 SPN。  
  
## <a name="verify-service-identities-in-wsdl"></a>验证 WSDL 中的服务标识  

 WS-SecurityPolicy 允许服务在元数据中发布有关其自身标识的信息。 通过 `svcutil` 或其他方法（如 <xref:System.ServiceModel.Description.WsdlImporter>）进行检索时，此标识信息将转换为 WCF 服务终结点地址的标识属性。 若客户端不验证这些服务标识是否正确、有效，则实际上是跳过了服务身份验证。 恶意服务可以通过更改其 WSDL 中声称的标识，利用此类客户端来执行凭据转发和其他“中间人”攻击。  
  
## <a name="use-x509-certificates-instead-of-ntlm"></a>使用 X509 证书而不是 NTLM  

 WCF 为对等身份验证提供两种机制：X509 证书（由对等通道使用）和 Windows 身份验证（其中 SSPI 协商从 Kerberos 降级为 NTLM）。  由于以下几个原因，使用 1024 位或更多位的密钥、基于证书的身份验证优于 NTLM：  
  
- 提供相互身份验证；  
  
- 使用更强的加密算法；以及  
  
- 不易使用转发的 X509 凭据。  

## <a name="always-revert-after-impersonation"></a>始终在模拟后还原  

 在使用启用客户端模拟的 API 时，应确保还原为原始标识。 例如，在使用 <xref:System.Security.Principal.WindowsIdentity> 和 <xref:System.Security.Principal.WindowsImpersonationContext> 时使用 C# `using` 语句或 Visual Basic `Using` 语句，如以下代码中所示。 <xref:System.Security.Principal.WindowsImpersonationContext> 类实现 <xref:System.IDisposable> 接口，因此，一旦代码离开 `using` 块，公共语言运行库 (CLR) 就会自动还原到原始标识。  
  
 [!code-csharp[c_SecurityBestPractices#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securitybestpractices/cs/source.cs#1)]
 [!code-vb[c_SecurityBestPractices#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securitybestpractices/vb/source.vb#1)]  
  
## <a name="impersonate-only-as-needed"></a>仅根据需要进行模拟  

 通过使用 <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> 类的 <xref:System.Security.Principal.WindowsIdentity> 方法，可以在控制极严的范围中使用模拟。 与此相反的是，使用 <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> 的 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性允许对整个操作的范围进行模拟。 只要有可能，应使用更精确的 <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> 方法来控制模拟的范围。  
  
## <a name="obtain-metadata-from-trusted-sources"></a>从受信任的源获取元数据  

 确保信任元数据的源，并确保没有人篡改元数据。 使用 HTTP 协议检索到的元数据是以明文形式发送的，可能被篡改。 如果服务使用 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> 和 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A> 属性，请根据服务创建者提供的 URL，使用 HTTPS 协议下载数据。  
  
## <a name="publish-metadata-using-security"></a>使用安全发布元数据  

 要防止篡改服务的已发布元数据，可使用传输或消息级安全来保证元数据交换终结点的安全。 有关详细信息，请参阅[发布元数据终结点](../publishing-metadata-endpoints.md)和[如何：使用代码发布服务的元数据](how-to-publish-metadata-for-a-service-using-code.md)。  
  
## <a name="ensure-use-of-local-issuer"></a>确保使用本地颁发者  

 如果为某个给定绑定指定了颁发者地址和绑定，则不对使用该绑定的终结点使用本地颁发者。 希望始终使用本地颁发者的客户应确保不使用这样的绑定，或修改绑定以使颁发者地址为 null。  
  
## <a name="saml-token-size-quotas"></a>SAML 令牌大小配额  

 如果在消息中序列化安全断言标记语言 (SAML) 令牌，无论这些令牌是由安全令牌服务 (STS) 颁发的，还是客户端将其作为身份验证的一部分提交给服务，最大消息大小配额都必须足够大，以便能够容纳 SAML 令牌和其他消息部分。 正常情况下，默认消息大小配额足够使用。 但是，当 SAML 令牌由于包含数以百计的声明而过于庞大时，您可能需要提高配额，以便容纳序列化的令牌。 有关配额的详细信息，请参阅[数据的安全注意事项](security-considerations-for-data.md)。  
  
## <a name="set-securitybindingelementincludetimestamp-to-true-on-custom-bindings"></a>将自定义绑定上的 SecurityBindingElement.IncludeTimestamp 设置为 True  

 创建自定义绑定时，必须将 <xref:System.ServiceModel.Channels.SecurityBindingElement.IncludeTimestamp%2A> 设置为 `true`。 否则如果将 <xref:System.ServiceModel.Channels.SecurityBindingElement.IncludeTimestamp%2A> 设置为 `false`，并且客户端使用基于非对称密钥的令牌（例如 X509 证书），则不会对消息进行签名。  
  
## <a name="see-also"></a>另请参阅

- [安全注意事项](security-considerations-in-wcf.md)
- [数据的安全考虑事项](security-considerations-for-data.md)
- [元数据的安全注意事项](security-considerations-with-metadata.md)
