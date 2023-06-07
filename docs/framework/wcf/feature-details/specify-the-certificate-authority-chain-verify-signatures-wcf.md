---
title: 如何：指定用于验证签名的证书颁发机构证书链 (WCF)
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], specifying the certificate authority certificate chain
- certificates [WCF], verifying signatures
ms.assetid: 7c719355-aa41-4567-80d0-5115a8cf73fd
ms.openlocfilehash: 0a03902c9a0d36ebd6e2c38f4a827737cacec447
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245027"
---
# <a name="how-to-specify-the-certificate-authority-certificate-chain-used-to-verify-signatures-wcf"></a>如何：指定用于验证签名的证书颁发机构证书链 (WCF)

如果 Windows Communication Foundation (WCF) 收到使用 x.509 证书签名的 SOAP 消息，则默认情况下，它会验证 x.509 证书是否由受信任的证书颁发机构颁发。 通过搜索证书存储区并确定是否已将该证书颁发机构的证书指定为受信任的证书，可以做到这一点。 为了使 WCF 作出此决定，必须在正确的证书存储中安装证书颁发机构证书链。  
  
### <a name="to-install-a-certification-authority-certificate-chain"></a>安装证书颁发机构证书链  
  
- 对于 SOAP 消息接收方打算信任其 x.509 证书的每个证书颁发机构，请将证书颁发机构证书链安装到 WCF 配置为从中检索 x.509 证书的证书存储中。  
  
     例如，如果 SOAP 消息接收方打算信任由 Microsoft 颁发的 x.509 证书，则必须将 Microsoft 的证书颁发机构证书链安装在 WCF 设置为从中查找 x.509 证书的证书存储区中。 可在代码或配置中指定 WCF 查找 x.509 证书的证书存储区。 例如，可以使用方法在代码中指定 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> ，也可以在配置中使用几种方法来指定，包括 [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) 。  
  
     因为 Windows 附带有一组默认的用于受信任证书颁发机构的证书链，所以可能不必为所有证书颁发机构安装证书链。  
  
    1. 导出证书颁发机构证书链。  
  
         具体导出方式取决于证书颁发机构。 如果证书颁发机构正在运行 Microsoft 证书服务，请选择 " **下载 ca 证书、证书链或 CRL**"，然后选择 " **下载 ca 证书**"。  
  
    2. 导入证书颁发机构证书链。  
  
         在 Microsoft 管理控制台 (MMC) 中，打开证书管理单元。 对于 WCF 配置为从中检索 x.509 证书的证书存储区，请选择 "**受信任的根****证书颁发机构**" 文件夹。 在 " **受信任的根证书颁发机构** " 文件夹下，右键单击 " **证书** " 文件夹，指向 " **所有任务**"，然后单击 " **导入**"。 提供在步骤 a 中导出的文件。  
  
         有关将证书管理单元与 MMC 一起使用的详细信息，请参阅 [如何：使用 Mmc 管理单元查看证书](how-to-view-certificates-with-the-mmc-snap-in.md)。  
  
## <a name="see-also"></a>另请参阅

- [使用证书](working-with-certificates.md)
