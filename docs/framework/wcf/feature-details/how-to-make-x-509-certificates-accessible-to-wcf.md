---
title: 如何：使 X.509 证书可由 WCF 访问
description: 了解如何使 x.509 证书可供 WCF 访问。 应用程序代码必须指定证书存储区的名称和位置。 可能还有其他要求。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- X.509 certificates [WCF]
- certificates [WCF], making X.509 certificates accessible to WCF
- X.509 certificates [WCF], making accessible to WCF
ms.assetid: a54e407c-c2b5-4319-a648-60e43413664b
ms.openlocfilehash: 06a6167f0ad352955eb6b764ef8bfdb1394f4ed9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279735"
---
# <a name="how-to-make-x509-certificates-accessible-to-wcf"></a>如何：使 X.509 证书可由 WCF 访问

为了使 x.509 证书可 Windows Communication Foundation (WCF) ，应用程序代码必须指定证书存储区的名称和位置。 在某些情况下，进程标识必须具有对包含私钥的文件的访问权限，此私钥与 X.509 证书相关联。 若要获取与证书存储区中的 x.509 证书关联的私钥，WCF 必须有权执行此操作。 默认情况下，只有所有者和“系统”帐户才可以访问证书的私钥。  
  
### <a name="to-make-x509-certificates-accessible-to-wcf"></a>使 X.509 证书可由 WCF 访问  
  
1. 向运行 WCF 的帐户提供对包含与 x.509 证书关联的私钥的文件的读取访问权限。  
  
    1. 确定 WCF 是否需要对 x.509 证书私钥的读取访问权限。  
  
         下表详细描述在使用某个 X.509 证书时是否必须提供私钥。  
  
        |X.509 证书用途|私钥|  
        |---------------------------|-----------------|  
        |对出站 SOAP 消息进行数字签名。|是|  
        |验证入站 SOAP 消息的签名。|否|  
        |对出站 SOAP 消息进行加密。|否|  
        |对入站 SOAP 消息进行解密。|是|  
  
    2. 确定在其中存储证书的存储区的位置和名称。  
  
         在其中存储证书的证书存储区要么在应用程序代码中指定，要么在配置中指定。 例如，下面的示例指定证书位于名为 `CurrentUser` 的 `My` 证书存储区中。  
  
         [!code-csharp[x509Accessible#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/x509accessible/cs/source.cs#1)]
         [!code-vb[x509Accessible#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/x509accessible/vb/source.vb#1)]  
  
    3. 使用 [FindPrivateKey](../samples/findprivatekey.md) 工具确定证书的私钥在计算机上的位置。  
  
         [FindPrivateKey](../samples/findprivatekey.md)工具需要证书存储名称、证书存储位置和唯一标识证书的内容。 此工具接受将证书的主题名称或其指纹作为唯一标识符。 有关如何确定证书指纹的详细信息，请参阅 [如何：检索证书的指纹](how-to-retrieve-the-thumbprint-of-a-certificate.md)。  
  
         下面的代码示例使用 [FindPrivateKey](../samples/findprivatekey.md) 工具确定存储区中的证书的私钥位置 `My` ，其指纹为 `CurrentUser` `46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d` 。  
  
        ```console
        findprivatekey.exe My CurrentUser -t "46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d" -a  
        ```  
  
    4. 确定 WCF 在其下运行的帐户。  
  
         下表详细说明了在给定方案下运行 WCF 时所依据的帐户。  
  
        |场景|进程标识|  
        |--------------|----------------------|  
        |客户端（控制台或 WinForms 应用程序）。|当前登录的用户。|  
        |自承载服务。|当前登录的用户。|  
        |托管在 IIS 6.0 (Windows Server 2003) 或 IIS 7.0 (Windows Vista) 中的服务。|网络服务|  
        |承载于 IIS 1.x (Windows XP) 中的服务。|由 Machine.config 文件中的 `<processModel>` 元素控制。 默认帐户为 ASPNET。|  
  
    5. 使用 icacls.exe 的工具向运行 WCF 的帐户授予对包含私钥的文件的读取访问权限。  
  
         下面的代码示例为指定文件 (DACL) 中编辑自由访问控制列表，以向网络服务帐户授予读取 (： R) 对文件的访问权限。  
  
        ```console
        icacls.exe "C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys\8aeda5eb81555f14f8f9960745b5a40d_38f7de48-5ee9-452d-8a5a-92789d7110b1" /grant "NETWORK SERVICE":R  
        ```  
  
## <a name="see-also"></a>另请参阅

- [FindPrivateKey](../samples/findprivatekey.md)
- [如何：检索证书的指纹](how-to-retrieve-the-thumbprint-of-a-certificate.md)
- [使用证书](working-with-certificates.md)
