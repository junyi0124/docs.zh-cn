---
title: 保证服务的安全
ms.date: 03/30/2017
helpviewer_keywords:
- configuration [WCF], securing services
- WCF security
- WCF, security
ms.assetid: f0ecc6f7-f4b5-42a4-9cb1-b02e28e26620
ms.openlocfilehash: f2a8e10aaf9c1dbe3065344963fcc712776cd2db
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242222"
---
# <a name="securing-services"></a>保证服务的安全

WCF) 服务 Windows Communication Foundation (的安全性包括两个主要要求：传输安全和授权。  (第三个要求，即审核安全事件，请参阅 [审核](./feature-details/auditing-security-events.md)。 ) 简而言之，传输安全包括身份验证 (验证服务和客户端) 的身份、机密性 (消息加密) 以及完整性 (数字签名来检测篡改) 。 授权是控制对资源的访问，例如仅允许特权用户读取文件。 使用 WCF 的功能，可以轻松地实现这两个主要需求。  
  
 除了 <xref:System.ServiceModel.BasicHttpBinding> 类 (或 [\<basicHttpBinding>](../configure-apps/file-schema/wcf/basichttpbinding.md) 配置) 中的元素，默认情况下，所有预定义绑定都启用了传输安全性。 本节中的主题包含两种基本方案：对 Internet 信息服务 (IIS) 上承载的 Intranet 服务实现传输安全和授权，以及对 IIS 上承载的服务实现传输安全和授权。  
  
> [!NOTE]
> Windows XP Home 不支持 Windows 身份验证。 因此，不应在该系统上运行服务。  
  
## <a name="security-basics"></a>安全基本知识  

 安全依赖于凭据 。 凭据用于证明实体是否属实。  (*实体* 可以是个人、软件流程、公司，也可以是可以授权的任何内容。 ) 例如，服务的客户端会 *声明**标识*，而凭据以某种方式证明该声明。 在典型方案中，会发生凭据交换。 首先，某服务将对其标识发出声明，并使用凭据向客户端证明该标识。 同样，客户端也将对某标识发出声明，并向该服务出具凭据。 如果双方都信任对方的凭据，则可以建立安全上下文  ，在此上下文中，将以保密方式对所有消息进行交换，并且所有消息都将进行签名以保护其完整性。 服务建立客户端的标识之后，它就可以将凭据中的声明与 *角色* 或组中的 *成员身份* 匹配。 在任何一种情况下，使用客户端所属的角色或组时，服务将授权  该客户端基于角色或组特权执行一组有限的操作。  
  
## <a name="windows-security-mechanisms"></a>Windows 安全机制  

 如果客户端和服务计算机都位于需要二者登录到网络的 Windows 域中，则凭据将由 Windows 基础结构来提供。 在这种情况下，计算机用户登录到网络时将会建立凭据。 必须验证网络上的每个用户和每台计算机，确认其属于一组受信任的用户和计算机。 在 Windows 系统中，受信任的用户和计算机也称为“安全主体” 。  
  
 在 Kerberos  控制器支持的 Windows 域中，Kerberos 控制器使用基于向每个安全主体授予票证的方案。 控制器授予的票证受系统中的其他票证授予者的信任。 实体在尝试执行某种操作或访问资源  （如计算机上的文件或目录）时，都将对票证的有效性进行检查；如果票证通过检查，则主体将再次被授予一个用于执行该操作的票证。 这种授予票证的方法比尝试对每项操作的主体都进行验证的备选方法更为高效。  
  
 Windows 域中使用的一种旧的机制是 NT LAN Manager (NTLM)，这种机制的安全性较低。 在无法使用 Kerberos 的情况下（通常是在 Windows 域之外，例如在工作组中），可以将 NTLM 用作备选方法。 NTLM 也可用作 IIS 的安全选项。  
  
 在 Windows 系统上，授权的工作方式是将每个计算机和用户分配到一个角色和组的集合中。 例如，每台 Windows 计算机必须由管理员 角色的人员（或一组人员）来进行设置和控制。 另外一个角色是用户 角色，该角色拥有一组受到很多约束的权限。 除了该角色以外，还可将用户分配到组中。 使用组，多个用户可以使用同一个角色来执行操作。 因此，Windows 计算机实际上是通过将用户分配到组中来进行管理的。 例如，可以将多个用户分配到计算机的用户组，可以将一组受到很多约束的用户分配到管理员组。 在本地计算机上，管理员还可以创建新组并将其他用户（甚至其他组）分配到该组。  
  
 在运行 Windows 的计算机上，可以对目录中的每个文件夹进行保护。 即，您可以选择文件夹，并控制哪些用户可以访问文件，以及这些用户是否可以复制文件、更改文件（在最大特权情况下）、删除文件或向文件夹中添加文件。 这称为访问控制，这种机制称为访问控制列表 (ACL)。 创建 ACL 时，可以为任何一个或多个组以及域中的单个成员分配访问特权。  
  
 WCF 基础结构设计为使用这些 Windows 安全机制。 所以，如果您要创建将在 Intranet 中部署的服务，并且仅限 Windows 域的成员使用该服务的客户端，则可以轻松实现安全。 仅有效用户才可登录到该域。 用户登录后，Kerberos 控制器将允许每个用户与任何其他计算机或应用程序建立安全上下文。 在本地计算机上，可以轻松创建组，并且在保护特定文件夹时，可以使用这些组来分配计算机的访问特权。  
  
## <a name="implementing-windows-security-on-intranet-services"></a>在 Intranet 服务上实现 Windows 安全  

 若要保证 Windows 域上以独占方式运行的应用程序的安全，可以使用 <xref:System.ServiceModel.WSHttpBinding> 或 <xref:System.ServiceModel.NetTcpBinding> 绑定的默认安全设置。 默认情况下，同一 Windows 域中的任何人都可以访问 WCF 服务。 由于这些用户已登录到网络，因此他们是受信任的。 服务与客户端之间的消息将会进行加密以保证保密性，并将进行签名以保证完整性。 有关如何创建使用 Windows 安全的服务的详细信息，请参阅 [如何：使用 Windows 凭据保护服务](how-to-secure-a-service-with-windows-credentials.md)。  
  
### <a name="authorization-using-the-principalpermissionattribute-class"></a>使用 PrincipalPermissionAttribute 类进行授权  

 如果需要限制对计算机上的资源的访问，最简单的方法是使用 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 类。 使用此属性，可以限制对服务操作的调用，方法是要求用户必须属于指定的 Windows 组或角色，或者必须是特定用户。 有关详细信息，请参阅 [如何：使用 PrincipalPermissionAttribute 类限制访问权限](how-to-restrict-access-with-the-principalpermissionattribute-class.md)。  
  
### <a name="impersonation"></a>模拟  

 模拟是可用于控制对资源的访问的另外一种机制。 默认情况下，由 IIS 承载的服务将会在 ASPNET 帐户的标识下运行。 ASPNET 帐户仅可访问其拥有权限的资源。 然而，可以对文件夹设置 ACL 以排除 ASPNET 服务帐户，但允许某些其他标识访问该文件夹。 那么问题将变为，如果不允许 ASPNET 帐户访问该文件夹，如何允许其他用户访问该文件夹。 答案是使用模拟，由此允许服务使用客户端的凭据来访问特殊资源。 另外一个示例是当访问仅某些用户拥有权限的 SQL Server 数据库时的情况。 有关使用模拟的详细信息，请参阅 [如何：在服务上模拟客户端](how-to-impersonate-a-client-on-a-service.md) 和 [委派和模拟](./feature-details/delegation-and-impersonation-with-wcf.md)。  
  
## <a name="security-on-the-internet"></a>Internet 上的安全  

 Internet 上的安全包含与 Intranet 上的安全相同的要求。 服务需要提供其凭据以证明其真实性，客户端需要向服务证明其标识。 在证明了客户端的标识之后，服务可以控制客户端拥有怎样的资源访问权限。 但是，由于 Internet 的特性不同，所提供的凭据与 Windows 域中使用的凭据会有所不同。 Kerberos 控制器可以使用凭据的票证处理域中用户的身份验证，而在 Internet 上，服务和客户端依赖于多种不同方法的任何一种来提供凭据。 然而，本主题的目的是提供一种通用方法，使您能够创建可在 Internet 上访问的 WCF 服务。  
  
### <a name="using-iis-and-aspnet"></a>使用 IIS 和 ASP.NET  

 Internet 安全的要求和用于解决这些问题的机制并不是什么新鲜事物。 IIS 是 Microsoft 针对 Internet 的 Web 服务器，并且有许多安全功能可解决这些问题;此外，ASP.NET 还包括 WCF 服务可以使用的安全功能。 若要利用这些安全功能，请在 IIS 上托管 WCF 服务。  
  
#### <a name="using-aspnet-membership-and-role-providers"></a>使用 ASP.NET 成员资格和角色提供程序  

 ASP.NET 包括成员资格和角色提供程序。 提供程序是一个用于对调用方进行身份验证的用户名/密码对数据库，该数据库还允许您指定每个调用方的访问特权。 使用 WCF，可以通过配置轻松地使用预先存在的成员资格和角色提供程序。 有关演示此内容的示例应用程序，请参阅 [Membership and Role Provider](./samples/membership-and-role-provider.md) 示例。  
  
### <a name="credentials-used-by-iis"></a>IIS 使用的凭据  

 与 Kerberos 控制器支持的 Windows 域不同，Internet 环境并没有一个控制器来管理随时登录的成百上千万的用户， 而是在其凭据中最常采用 X.509 证书（也称为安全套接字层 (SSL) 证书）的形式。 这些证书通常由证书颁发机构 颁发，证书颁发机构可以是担保证书以及将向其颁发证书的人员的真实性的第三方公司。 若要在 Internet 上公开服务，还必须提供这样一个受信任的证书来对服务进行身份验证。  
  
 此时将出现一个问题，如何才能获取这样的证书呢？ 如果您准备部署服务并购买针对该服务的证书，则获取证书的一种方法是向第三方证书颁发机构（如 Authenticode 或 VeriSign）购买。 但是，如果你正在使用 WCF 的开发阶段，并且尚未准备好购买证书，则可以使用用于创建 x.509 证书的工具和技术来模拟生产部署。 有关详细信息，请参阅使用 [证书](./feature-details/working-with-certificates.md)。  
  
## <a name="security-modes"></a>安全模式  

 WCF 安全编程需要几个关键决策点。 最基本的一个决策点是选择安全模式 。 两个主要的安全模式是传输模式  和消息模式 。  
  
 第三种模式是带有消息凭据的传输模式，它结合了前两种主要模式的语义 。  
  
 安全模式确定如何保证消息的安全，每一种选择都有利有弊，如下所述。 有关设置安全模式的详细信息，请参阅 [如何：设置安全模式](how-to-set-the-security-mode.md)。  
  
#### <a name="transport-mode"></a>传输模式  

 网络与应用程序之间存在多个层。 其中一个是 *传输* 层 *，* 用于管理终结点之间的消息传输。 出于当前目的，只需了解 WCF 使用多个传输协议，其中每个协议都可以保护消息传输的安全。  (有关传输的详细信息，请参阅 [传输](./feature-details/transports.md)。 )   
  
 常用的协议是 HTTP，还有 TCP。 这些协议中的每一个都可以通过特定于该协议的一个或多个机制来保证消息传输的安全。 例如，HTTP 协议使用基于 HTTP 的 SSL（通常缩写为“HTTPS”）来保证安全。 因而，当选择传输模式来保证安全时，您要选择使用该协议所指示的机制。 例如，如果您选择 <xref:System.ServiceModel.WSHttpBinding> 类并将其安全模式设置为“传输”，则您要选择基于 HTTP 的 SSL (HTTPS) 作为安全机制。 传输模式的好处在于它比消息模式更为高效，原因是其安全是在相对较低的级别进行集成的。 使用传输模式时，必须根据传输规范实现安全机制，这样消息才能通过传输在各点之间安全流动。  
  
#### <a name="message-mode"></a>消息模式  

 相比之下，消息模式通过在每条消息中都包含安全数据来提供安全保障。 使用 XML 和 SOAP 安全标头时，每条消息中都包含确保消息的完整性和保密性所需要的凭据及其他数据。 每条消息都包含安全数据时，由于必须逐一处理每条消息，因此将导致性能下降。 在传输模式下 (，一旦保护了传输层，所有消息就会自由地流动。但 ) 不过，消息安全具有比传输安全更灵活的优点。 即，安全要求不由传输协议确定。 您可以使用任何类型的客户端凭据来保证消息的安全 （在传输模式下，传输协议将确定您可以使用的客户端凭据的类型）。  
  
#### <a name="transport-with-message-credentials"></a>带有消息凭据的传输  

 此第三种模式集传输安全和消息安全的长处于一身。 在此模式下，传输安全用于有效确保每条消息的保密性和完整性。 同时，每条消息都包含其凭据数据，这使得可以对消息进行身份验证。 通过身份验证，还可以实现授权。 通过对发送方进行身份验证，可以根据发送方的标识来授予（或拒绝）对资源的访问权限。  
  
## <a name="specifying-the-client-credential-type-and-credential-value"></a>指定客户端凭据类型和凭据值  

 选择安全模式后，可能还需要指定客户端凭据类型。 客户端凭据类型指定客户端必须使用哪种类型将其自身向服务器进行身份验证。  
  
 但是，并非所有方案都要求客户端凭据类型。 使用基于 HTTP 的 SSL (HTTPS) 时，服务将其自身向客户端进行身份验证。 作为此身份验证的一部分，服务的证书将在称为“协商” 的过程中发送到客户端。 通过 SSL 保证安全的传输将确保所有消息都是保密的。  
  
 如果您要创建要求对客户端进行身份验证的服务，则客户端凭据类型的选择取决于选定的传输和模式。 例如，使用 HTTP 传输并选择传输模式将使您可以有多种选择，例如 Basic、Digest 及其他类型。  (有关这些凭据类型的详细信息，请参阅 [了解 HTTP 身份验证](./feature-details/understanding-http-authentication.md)。 )   
  
 如果您要在 Windows 域中创建服务，且该服务将仅对其他网络用户可用，则最容易使用的是 Windows 客户端凭据类型。 但是，您也可能需要向服务提供证书。 [How to: Specify Client Credential Values](how-to-specify-client-credential-values.md)中对此进行了说明。  
  
#### <a name="credential-values"></a>凭据值  

 “凭据值”  是服务使用的实际凭据。 一旦指定了凭据类型，则还可能需要为服务配置实际的凭据。 如果您选择的是 Windows（且服务将在 Windows 域中运行），则不用指定实际的凭据值。  
  
## <a name="identity"></a>标识  

 在 WCF 中，术语 " *标识* " 对于服务器和客户端具有不同的含义。 简言之，在运行服务时，标识将在通过身份验证后分配到安全上下文。 若要查看实际的标识，请检查 <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> 类的 <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> 和 <xref:System.ServiceModel.ServiceSecurityContext> 属性。 有关详细信息，请参阅 [如何：检查安全上下文](how-to-examine-the-security-context.md)。  
  
 而在客户端上，标识用于验证服务。 在设计时，客户端开发人员可以将 [\<identity>](../configure-apps/file-schema/wcf/identity.md) 元素设置为从服务获取的值。 在运行时，客户端将对照服务的实际标识检查元素的值。 如果检查失败，则客户端将终止通信。 如果服务在特定的用户标识下运行，该值可以是用户主体名称 (UPN)；而如果服务在计算机帐户下运行，则该值可以是服务主体名称 (SPN)。 有关详细信息，请参阅 [服务标识和身份验证](./feature-details/service-identity-and-authentication.md)。 凭据也可以是证书或者在证书中找到的用于标识该证书的字段。  
  
## <a name="protection-levels"></a>保护级别  

 `ProtectionLevel` 属性出现在多个特性类（如 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.OperationContractAttribute> 类）中。 保护级别是一个值，它指定了支持服务的消息（或消息部分）是进行签名、签名并加密，还是未经签名或加密即发送。 有关属性的详细信息，请参阅 [了解保护级别](understanding-protection-level.md)和编程示例的详细信息，请参阅 [如何：设置 ProtectionLevel 属性](how-to-set-the-protectionlevel-property.md)。 有关使用中的上下文设计服务协定的详细信息 `ProtectionLevel` ，请参阅 [设计服务协定](designing-service-contracts.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>
- [服务标识和身份验证](./feature-details/service-identity-and-authentication.md)
- [了解保护级别](understanding-protection-level.md)
- [委托和模拟](./feature-details/delegation-and-impersonation-with-wcf.md)
- [设计服务协定](designing-service-contracts.md)
- [安全性](./feature-details/security.md)
- [安全性概述](./feature-details/security-overview.md)
- [如何：设置 ProtectionLevel 属性](how-to-set-the-protectionlevel-property.md)
- [如何：使用 Windows 凭据保护服务的安全](how-to-secure-a-service-with-windows-credentials.md)
- [如何：设置安全模式](how-to-set-the-security-mode.md)
- [如何：指定客户端凭据类型](how-to-specify-the-client-credential-type.md)
- [如何：使用 PrincipalPermissionAttribute 类限制访问](how-to-restrict-access-with-the-principalpermissionattribute-class.md)
- [如何：在服务上模拟客户端](how-to-impersonate-a-client-on-a-service.md)
- [如何：检查安全性上下文](how-to-examine-the-security-context.md)
