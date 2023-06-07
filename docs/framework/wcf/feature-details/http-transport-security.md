---
title: HTTP 传输安全
ms.date: 03/30/2017
ms.assetid: d3439262-c58e-4d30-9f2b-a160170582bb
ms.openlocfilehash: 1af9913ac977b0e1c112ca818a04842af9f1307c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280554"
---
# <a name="http-transport-security"></a>HTTP 传输安全

如果使用 HTTP 作为传输，则由安全套接字层 (SSL) 实现提供安全。 SSL 广泛用于 Internet 中，以便向客户端证明服务的身份，并且随后向通道提供保密性（加密）。 本主题说明 SSL 如何工作，以及如何在 Windows Communication Foundation (WCF) 中实现它。  
  
## <a name="basic-ssl"></a>基本 SSL  

 SSL 的工作方式可以通过一个典型方案得到最好的说明，在本示例中，该方案为银行的网站。 该网站允许客户使用用户名和密码登录。 在经过身份验证之后，用户可以执行事务，例如查看帐户余额、支付帐单以及将钱从一个帐户转到其他帐户。  
  
 当用户首次访问该站点时，SSL 机制会开始一系列协商（称为 *握手*），用户的客户端 (在本例中为 Internet Explorer) 。 SSL 首先向客户证明银行网站的身份。 这一步骤是必需的，因为客户首先必须知道他们正在与真实网站进行通信，而不是与一个试图引诱他们键入自己的用户名和密码的诈骗网站进行通信。 SSL 通过使用由受信任的颁发机构（例如 VeriSign）提供的 SSL 证书来执行此身份验证。 其逻辑如下：VeriSign 担保该银行网站的身份是真实的。 由于 Internet Explorer 信任 VeriSign，因此也信任该网站。 如果您希望向 VeriSign 进行核实，可以通过单击 VeriSign 徽标执行此操作。 这将显示一份含有到期日期以及接受方（银行网站）的真实性声明。  
  
 若要启动一个安全会话，客户端向服务器发送一个等效于“你好”的项，连同一个它可以用来签名、生成哈希以及进行加密和解密的加密算法列表。 作为响应，该网站发送回一个确认以及它对算法套件之一的选择。 在该初次握手期间，双方都发送和接收 Nonce。 *Nonce* 是随机生成的数据片段，与站点的公钥结合使用，用于创建哈希。 *哈希* 是使用标准算法（如 SHA1）从两个数字派生的新编号。  (客户端和站点还交换消息以同意要使用的哈希算法。 ) 哈希是唯一的，仅用于客户端与站点之间的会话对消息进行加密和解密。 客户端和服务都具有原始 Nonce 和证书的公钥，所以通信两端可以生成同一个哈希。 因此，客户端可以通过以下方式验证服务所发送的哈希：(a) 使用商定的算法根据数据计算哈希；并 (b) 将计算出的哈希与服务所发送的哈希进行比较。如果二者匹配，则客户端可以确信该哈希未遭篡改。 客户端随后可以将此哈希用作密钥，以便对同时包含另一个新哈希的消息进行加密。 服务可以使用此哈希对消息进行解密，并重新获得倒数第二个哈希。 这样，通信双方就都获知累积的信息（Nonce、公钥和其他数据），并且可以创建最后一个哈希（也称主密钥）。 这个最终的密钥使用倒数第二个哈希加密后发送。 然后，使用主密钥对会话的其余部分的消息进行加密和解密。 因为客户端和服务都使用相同的密钥，所以这也称为 *会话密钥*。  
  
 会话密钥还被描述为对称密钥，或“共享秘密”。 具有对称密钥很重要，因为它减少了事务双方所需执行的计算量。 如果每个消息都要求对 Nonce 和哈希进行新的交换，那么性能将会下降。 因此，SSL 的最终目标是使用允许消息在通信双方之间自由流动的对称密钥，同时具有更高程度的安全和效率。  
  
 因为协议可能因网站而异，所以前面的描述只是所发生过程的简化版本。 还有一种可能，就是客户端和网站都在握手期间生成在算法上相结合的 Nonce，以增加数据交换过程的复杂性，从而为该过程提供更多的保护。  
  
### <a name="certificates-and-public-key-infrastructure"></a>证书和公钥基础结构  

 在握手期间，服务还将其 SSL 证书发送到客户端。 该证书包含一些信息，例如证书的到期日期、颁发机构以及网站的统一资源标识符 (URI)。 客户端将该 URI 与它原来联系的 URI 进行比较，以确保二者匹配，并且对日期和颁发机构进行检查。  
  
 每个证书都有两个密钥：一个私钥和一个公钥，这两个密钥称为 *交换密钥对*。 简言之，只有证书的所有者知道私钥，而公钥则可以从证书中读取。 这两个密钥都可用来加密和解密摘要、哈希或其他密钥，但它们只能用于相反的操作。 例如，如果客户端使用公钥加密，则只有网站可以使用私钥对消息进行解密。 同样，如果网站使用私钥加密，则客户端可以使用公钥解密。 这可以使客户端确信只与私钥的拥有者交换消息，因为只有使用私钥加密的消息才可以使用公钥进行解密。 而网站可以确信它正在与已经使用公钥加密的客户端交换消息。 然而，这种交换仅对初次握手是安全的，因此在创建实际的对称密钥时要复杂得多。 尽管如此，所有通信都依赖于服务具有有效的 SSL 证书。  
  
## <a name="implementing-ssl-with-wcf"></a>使用 WCF 实现 SSL  

 HTTP 传输安全 (或 SSL) 是与 WCF 的外部提供的。 可以使用两种方式之一来实现 SSL；决定因素是承载应用程序的方式：  
  
- 如果使用 Internet Information Services (IIS) 作为 WCF 主机，请使用 IIS 基础结构来设置 SSL 服务。  
  
- 如果要创建自承载的 WCF 应用程序，可以使用 HttpCfg.exe 工具将 SSL 证书绑定到该地址。  
  
### <a name="using-iis-for-transport-security"></a>使用 IIS 实现传输安全  
  
#### <a name="iis-70"></a>IIS 7.0  

 若要将 IIS 7.0 设置为使用 SSL) 的安全主机 (，请参阅 [在 IIS 7.0 中配置安全套接字层](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771438(v=ws.10))。  
  
若要配置用于 IIS 7.0 的证书，请参阅 [在 iis 7.0 中配置服务器证书](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10))。  
  
#### <a name="iis-60"></a>IIS 6.0  

 若要将 IIS 6.0 设置为使用 SSL) 的安全主机 (，请参阅 [配置安全套接字层](/previous-versions/windows/it-pro/windows-server-2003/cc736992(v=ws.10))。  
  
 若要配置用于 IIS 6.0 的证书，请参阅 [Certificates_IIS_SP1_Ops](/previous-versions/windows/it-pro/windows-server-2003/cc757474(v=ws.10))。  
  
### <a name="using-httpcfg-for-ssl"></a>将 HttpCfg 用于 SSL  

 如果要创建自承载的 WCF 应用程序，请使用 [HttpCfg.exe](/windows/win32/http/httpcfg-exe) 工具。
  
 有关使用 HttpCfg.exe 工具设置 x.509 证书的端口的详细信息，请参阅 [如何：使用 SSL 证书配置端口](how-to-configure-a-port-with-an-ssl-certificate.md)。  
  
## <a name="see-also"></a>另请参阅

- [传输安全](transport-security.md)
- [消息安全](message-security-in-wcf.md)
