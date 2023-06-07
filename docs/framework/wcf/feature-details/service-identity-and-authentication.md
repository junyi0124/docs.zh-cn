---
title: 服务标识和身份验证
description: 了解服务的终结点标识，这是一个从服务 WSDL 生成的值，WCF 使用该值对服务进行身份验证。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- authentication [WCF], specifying the identity of a service
ms.assetid: a4c8f52c-5b30-45c4-a545-63244aba82be
ms.openlocfilehash: f1c1d41f3d2ebc4482fa7e5f28fcbefe88bb9a02
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253903"
---
# <a name="service-identity-and-authentication"></a>服务标识和身份验证

服务的 *终结点标识* 是从服务 Web 服务描述语言 (WSDL) 生成的值。 此值可传播到任何客户端，用于对服务进行身份验证。 在客户端启动与终结点的通信并且服务向客户端验证自己的身份之后，客户端将终结点标识值与终结点身份验证过程返回的实际值进行比较。 如果二者匹配，则客户端确信其已与预期的服务终结点联系。 这可以防止将客户端重定向到由恶意服务托管的终结点，从而防范 *网络钓鱼* 。  
  
 有关演示标识设置的示例应用程序，请参阅 [服务标识示例](../samples/service-identity-sample.md)。 有关终结点和终结点地址的详细信息，请参阅 [地址](endpoint-addresses.md)。  
  
> [!NOTE]
> 在使用 NT LanMan (NTLM) 进行身份验证时，将不检查服务标识，这是因为在 NTLM 下客户端无法对服务器进行身份验证。 在计算机是 Windows 工作组的一部分时，或者在运行不支持的 Kerberos 身份验证的 Windows 早期版本时，将使用 NTLM。  
  
 当客户端启动安全通道以向服务发送消息时，Windows Communication Foundation (WCF) 基础结构对服务进行身份验证，并且仅当服务标识与客户端使用的终结点地址中指定的标识匹配时才发送消息。  
  
 标识处理由以下几个阶段组成：  
  
- 在设计时，客户端开发人员确定终结点元数据（通过 WSDL 公开）的服务终结点。  
  
- 在运行时，客户端应用程序在向服务发送任何消息之前检查服务安全凭据的声明。  
  
 客户端上的标识处理与服务上的客户端身份验证相似。 直到已经对客户端的凭据进行了身份验证，安全服务才会执行代码。 同样，直到基于事先从服务元数据已知的内容对服务凭据进行了身份验证，客户端才会向服务发送消息。  
  
 <xref:System.ServiceModel.EndpointAddress.Identity%2A> 类的 <xref:System.ServiceModel.EndpointAddress> 属性表示由客户端调用的服务的标识。 服务在其元数据中发布 <xref:System.ServiceModel.EndpointAddress.Identity%2A>。 当客户端开发人员运行配置的 [元数据实用工具工具时 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 针对服务终结点，生成的配置包含服务的属性的值 <xref:System.ServiceModel.EndpointAddress.Identity%2A> 。 如果配置了安全) ，WCF 基础结构 (验证该服务是否拥有指定的标识。  
  
> [!IMPORTANT]
> 元数据包含服务的预期标识，因此建议以安全方式公开服务元数据，例如，通过创建服务的 HTTPS 终结点。 有关详细信息，请参阅 [如何：保护元数据终结点](how-to-secure-metadata-endpoints.md)。  
  
## <a name="identity-types"></a>标识类型  

 服务可以提供六种类型的标识。 每种标识类型对应于一个可以包含在配置中的 `<identity>` 元素内的元素。 使用的类型取决于方案和服务的安全要求。 下表描述每种标识类型。  
  
|标识类型|描述|典型的方案|  
|-------------------|-----------------|----------------------|  
|域名系统 (DNS)|将此元素用于 X.509 证书或 Windows 帐户。 它将凭据中指定的 DNS 名称与此元素中指定的值进行比较。|DNS 检查让您可以通过 DNS 或使用者名称来使用证书。 如果使用同一个 DNS 或使用者名称来重新颁发证书，则标识检查仍然有效。 在重新颁发证书时，它会获取新的 RSA 密钥，但保留相同的 DNS 或使用者名称。 这意味着客户端不必更新其有关服务的标识信息。|  
|证书。 `ClientCredentialType` 设置为 Certificate 时的默认值。|此元素指定要与客户端进行比较的 Base64 编码的 X.509 证书值。<br /><br /> 当使用 CardSpace 作为凭据对服务进行身份验证时，也使用此元素。|此元素将身份验证限制为单个基于其指纹值的证书。 这样将启用更为严格的身份验证，因为指纹值是唯一的。 这也带来一个需要注意的问题：如果使用相同使用者名称重新颁发证书，则证书也有一个新的指纹。 因此，客户端无法验证服务，除非新的指纹是已知的。 有关查找证书指纹的详细信息，请参阅 [如何：检索证书的指纹](how-to-retrieve-the-thumbprint-of-a-certificate.md)。|  
|证书引用|与前面描述的“证书”选项完全相同。 但是，此元素可让您指定证书名称并存储检索证书的位置。|与前面描述的“证书”方案相同。<br /><br /> 其优点是证书存储位置可以更改。|  
|RSA|此元素指定要与客户端进行比较的 RSA 密钥值。 这与“证书”选项类似，但并不使用证书的指纹，而是使用证书的 RSA 密钥。|RSA 检查可让您明确地将身份验证限制为单个证书（基于其 RSA 证书）。 这样将启用更为严格的特定 RSA 密钥身份验证，不过与此对应的代价是，如果更改 RSA 密钥值，则该服务不可再用于现有客户端。|  
|用户主体名称 (UPN)。 `ClientCredentialType` 设置为 Windows 时的默认值，并且服务进程不是在一个系统帐户下运行的。|此元素指定运行服务所依据的 UPN。 请参阅 [重写服务标识](../extending/overriding-the-identity-of-a-service-for-authentication.md)的 Kerberos 协议和标识部分进行身份验证。|这将确保服务在特定的 Windows 用户帐户下运行。 用户帐户可以是当前已登录用户，也可以是在特定用户帐户下运行的服务。<br /><br /> 如果服务是在 Active Directory 环境内的域帐户下运行，则此设置将利用 Windows Kerberos 安全。|  
|服务主体名称 (SPN)。 `ClientCredentialType` 设置为 Windows 时的默认值，并且服务进程是在一个系统帐户（LocalService、LocalSystem 或 NetworkService）下运行的。|此元素指定与服务的帐户相关联的 SPN。 请参阅 [重写服务标识](../extending/overriding-the-identity-of-a-service-for-authentication.md)的 Kerberos 协议和标识部分进行身份验证。|这将确保 SPN 和与 SPN 相关联的特定 Windows 帐户标识服务。<br /><br /> 可以使用 Setspn.exe 工具将计算机帐户与服务的用户帐户进行关联。<br /><br /> 如果服务是在一个系统帐户下或具有与其关联的 SPN 名称的域帐户下运行的，并且计算机是 Active Directory 环境中的域的一个成员，则此设置将利用 Windows Kerberos 安全。|  
  
## <a name="specifying-identity-at-the-service"></a>在服务上指定标识  

 通常情况下不需要设置服务上的标识，因为客户端凭据类型的选择即规定了服务元数据中公开的标识的类型。 有关如何重写或指定服务标识的详细信息，请参阅 [重写服务标识以便进行身份验证](../extending/overriding-the-identity-of-a-service-for-authentication.md)。  
  
## <a name="using-the-identity-element-in-configuration"></a>\<identity>在配置中使用元素  

 如果将前面演示的绑定中的客户端凭据类型更改为 `Certificate,`，则生成的 WSDL 将包含一个 Base64 序列化 X.509 证书作为标识值，如下面的代码所示。 这是除 Windows 之外的所有客户端凭据类型的默认值。  

 可以通过 `<identity>` 在配置中使用元素或在代码中设置标识，来更改默认服务标识的值或更改标识的类型。 下面的配置代码使用值 `contoso.com` 设置域名系统 (DNS) 标识。  

## <a name="setting-identity-programmatically"></a>以编程方式设置标识  

 服务无需显式指定标识，因为 WCF 会自动确定标识。 不过，WCF 允许你在终结点上指定标识（如果需要）。 下面的代码使用特定的 DNS 标识添加了一个新的服务终结点。  
  
 [!code-csharp[C_Identity#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_identity/cs/source.cs#5)]
 [!code-vb[C_Identity#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_identity/vb/source.vb#5)]  
  
## <a name="specifying-identity-at-the-client"></a>在客户端上指定标识  

 在设计时，客户端开发人员通常使用 " [)  (" 的元数据实用工具工具 ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 来生成客户端配置。 生成的配置文件（适用于客户端使用）包含服务器的标识。 例如，下面的代码是从指定 DNS 标识的服务生成的，如前面的示例中所示。 请注意，客户端的终结点标识值与服务的标识值匹配。 在此情况下，客户端接收服务的 Windows (Kerberos) 凭据，期望的值为 `contoso.com`。  

 如果接收的不是 Windows 凭据，则服务指定作为客户端凭据类型的证书，然后证书的 DNS 属性的值应为 `contoso.com`。 （或者，如果 DNS 属性为 `null`，则证书的使用者名称必须是 `contoso.com`。）  
  
#### <a name="using-a-specific-value-for-identity"></a>使用特定的标识值  

 下面的客户端配置文件演示了服务的标识如何应为一个特定的值。 在下面的示例中，客户端可以与两个终结点进行通信。 第一个终结点使用证书指纹进行标识，第二个终结点使用证书 RSA 密钥进行标识。 即，一个只包含一个公钥/私钥对、但不是由受信任的颁发机构颁发的证书。  

## <a name="identity-checking-at-run-time"></a>运行时标识检查  

 在设计时，客户端开发人员通过服务的元数据确定其标识。 在运行时，在服务上调用任何终结点之前执行标识检查。  
  
 标识值与元数据指定的身份验证类型相关联；换句话说，标识值与用于服务的凭据的类型相关联。  
  
 如果配置通道以将消息级或传输级安全套接字层 (SSL) 与用于身份验证的 X.509 证书一起用来进行身份验证，则下面的标识值是有效的：  
  
- DNS。 WCF 确保 SSL 握手期间提供的证书包含 `CommonName` 与客户端上的 dns 标识中指定的值相等的 DNS 或 (CN) 属性。 请注意，除了确定服务器证书的有效性之外还执行这些检查。 默认情况下，WCF 将验证服务器证书是否由受信任的根证书颁发机构颁发。  
  
- 证书。 在 SSL 握手期间，WCF 可确保远程终结点提供标识中指定的确切证书值。  
  
- 证书引用。 与“证书”相同。  
  
- RSA。 在 SSL 握手期间，WCF 可确保远程终结点提供标识中指定的确切 RSA 密钥。  
  
 如果服务将消息级或传输级 SSL 与用于身份验证的 Windows 凭据一起用来进行身份验证，并且协商凭据，则下面的标识值是有效的：  
  
- DNS。 协商传递服务的 SPN，因此可以检查 DNS 名称。 SPN 采用 `host/<dns name>` 的格式。  
  
- SPN。 返回显式的服务 SPN，例如 `host/myservice`。  
  
- UPN。 服务帐户的 UPN。 UPN 的格式为 `username` @ `domain` 。 例如，当服务在用户帐户中运行时，它可以是 `username@contoso.com`。  
  
 以编程的方式指定标识（使用 <xref:System.ServiceModel.EndpointAddress.Identity%2A> 属性）是可选的。 如果未指定标识，并且客户端凭据类型是 Windows，则默认使用 SPN 并将其设置为以“host/”文本为前缀的服务终结点地址的主机名部分。 如果未指定标识，并且客户端凭据类型是证书，则默认使用 `Certificate`。 这将应用于消息级安全和传输级安全。  
  
## <a name="identity-and-custom-bindings"></a>标识和自定义绑定  

 因为服务的标识取决于所使用的绑定类型，所以应确保在创建自定义绑定时公开相应的标识。 例如，在下面的代码示例中，公开的标识与安全类型不兼容，因为安全对话引导绑定的标识与终结点上的绑定标识不匹配。 在 <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement> 设置 UPN 或 SPN 标识时，安全对话绑定设置 DNS 标识。  
  
 [!code-csharp[C_Identity#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_identity/cs/source.cs#8)]
 [!code-vb[C_Identity#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_identity/vb/source.vb#8)]  
  
 有关如何正确地为自定义绑定堆栈绑定元素的详细信息，请参阅 [创建 User-Defined 绑定](../extending/creating-user-defined-bindings.md)。 有关使用创建自定义绑定的详细信息 <xref:System.ServiceModel.Channels.SecurityBindingElement> ，请参阅 [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)。  
  
## <a name="see-also"></a>另请参阅

- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
- [如何：创建自定义客户端标识验证工具](../extending/how-to-create-a-custom-client-identity-verifier.md)
- [选择凭据类型](selecting-a-credential-type.md)
- [使用证书](working-with-certificates.md)
- [ServiceModel 元数据实用工具 (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
- [创建用户定义的绑定](../extending/creating-user-defined-bindings.md)
- [如何：检索证书的指纹](how-to-retrieve-the-thumbprint-of-a-certificate.md)
