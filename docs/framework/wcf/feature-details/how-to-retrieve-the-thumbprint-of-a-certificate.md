---
title: 如何：检索证书的指纹
description: 了解如何指定在 x.509 证书中找到的声明，这在开发使用证书进行身份验证的 WCF 应用程序时是必需的。
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], retrieving thumbprint
ms.assetid: da3101aa-78cd-4c34-9652-d1f24777eeab
ms.openlocfilehash: 1ecefdfe88426afa8e2d3d8eea758e7decf19ed8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249821"
---
# <a name="how-to-retrieve-the-thumbprint-of-a-certificate"></a>如何：检索证书的指纹

编写使用 x.509 证书进行身份验证的 Windows Communication Foundation (WCF) 应用程序时，通常需要指定在证书中找到的声明。 例如，在 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint> 方法中使用 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> 枚举时，必须提供指纹声明。 查找声明值有两个步骤。 首先，打开用于证书的 Microsoft 管理控制台 (MMC) 管理单元  (参阅 [如何：使用 MMC 管理单元查看证书](how-to-view-certificates-with-the-mmc-snap-in.md)。 ) 秒钟，如此处所述，查找相应的证书并将其指纹 (或其他声明值) 。  
  
 如果您要将证书用于服务身份验证，一定要记下 **“颁发给”** 列（控制台中的首列）的值，这一点很重要。 在将安全套接字层 (SSL) 用作传输安全时，首先需要执行的检查之一是将服务的基址统一资源标识符 (URI) 与 **“颁发给”** 值进行比较。 这些值必须匹配，否则身份验证进程会暂停。  
  
 你还可以使用 PowerShell New-SelfSignedCertificate cmdlet 来创建只在开发过程中使用的临时证书。 但是，默认情况下，此类证书不是由证书颁发机构颁发的，并且不可用于生产目的。 有关详细信息，请参阅 [如何：创建在开发期间使用的临时证书](how-to-create-temporary-certificates-for-use-during-development.md)。  
  
### <a name="to-retrieve-a-certificates-thumbprint"></a>检索证书的指纹  
  
1. 打开用于证书的 Microsoft 管理控制台 (MMC) 管理单元 （请参阅 [How to: View Certificates with the MMC Snap-in](how-to-view-certificates-with-the-mmc-snap-in.md)。）  
  
2. 在 **“控制台根节点”** 窗口的左窗格中，单击 **“证书(本地计算机)”**。  
  
3. 单击 **“个人”** 文件夹将其展开。  
  
4. 单击 **“证书”** 文件夹将其展开。  
  
5. 在证书列表中，注意 **“预期目的”** 标题。 查找一个按预期目的列出 **“客户端身份验证”** 的证书。  
  
6. 双击该证书。  
  
7. 在 **“证书”** 对话框中，单击 **“详细信息”** 选项卡。  
  
8. 浏览该字段列表并单击 **“指纹”**。  
  
9. 复制该框中的十六进制字符。 如果此指纹在 `X509FindType`的代码中使用，请移除十六进制数字之间的空格。 例如，指纹“a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2a 7b”在代码中应指定为“a909502dd82ae41433e6f83886b00d4277a32a7b”。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A>
- [如何：使用 SSL 证书配置端口](how-to-configure-a-port-with-an-ssl-certificate.md)
- [如何：使用 MMC 管理单元查看证书](how-to-view-certificates-with-the-mmc-snap-in.md)
- [如何：创建开发期间使用的临时证书](how-to-create-temporary-certificates-for-use-during-development.md)
