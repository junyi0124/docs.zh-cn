---
title: 如何：获取证书 (WCF)
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], obtaining
ms.assetid: d53762fd-15ea-42dc-b0ea-6a6597aa23f7
ms.openlocfilehash: 20e936feb5839a7e71f6579583d558abfc2fd9cf
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96234487"
---
# <a name="how-to-obtain-a-certificate-wcf"></a>如何：获取证书 (WCF)

若要使用 Windows Communication Foundation 的任何 (WCF) 功能，只需获取证书即可。  
  
### <a name="to-obtain-an-x509-certificate"></a>获取 X.509 证书  
  
1. 选择以下选项之一：  
  
    - 从证书颁发机构（如 VeriSign Inc）购买证书。  
  
    - 设置自己的证书服务，并让证书颁发机构为证书签名。 Windows Server 2003、Windows 2000 Server、Windows 2000 Server Datacenter 和 Windows 2000 Datacenter 服务器都包含支持公钥基础结构 (PKI) 的证书服务。 在 Windows Server 2008 中，使用 [Active Directory 证书服务](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731564(v=ws.10)) 角色来管理证书颁发机构。  
  
    - 设置自己的证书服务，但不对证书进行签名。  
  
    > [!NOTE]
    > 无论采取哪种方法，包含 X.509 证书的 SOAP 请求的接收方都必须信任 X.509 证书。 这意味着证书链中的 X.509 证书或颁发者位于“受信任的人”证书存储区中，并且 X.509 证书不在“不受信任的证书”存储区中。  
  
## <a name="see-also"></a>另请参阅

- [使用证书](working-with-certificates.md)
- [如何：创建开发期间使用的临时证书](how-to-create-temporary-certificates-for-use-during-development.md)
