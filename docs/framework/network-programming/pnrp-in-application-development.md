---
title: 应用程序开发中的 PNRP
ms.date: 03/30/2017
ms.assetid: 265615d6-4423-4b5d-8626-752e456f4f4e
ms.openlocfilehash: 7c59b2be8384c8f8cc6bbdc4546a678cfe1c538d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263186"
---
# <a name="pnrp-in-application-development"></a>应用程序开发中的 PNRP

Windows Vista 中，网络应用程序可以通过简化的 PNRP 应用程序编程接口 (API) 访问名称发布和解析函数。  
  
## <a name="implementing-the-peer-name-resolution-protocol"></a>执行对等名称解析协议  

 借助简化的 PNRP API，无需显式指定云注册名称和地址，PNRP 组件自动决定要加入的合适的云以及要在云中发布的地址。  
  
 对于 Windows Vista 中高度简化的 PNRP 名称解析, PNRP 名称现已集成到 getaddrinfo() Windows 套接字函数。 若要使用 PNRP 将名称解析为 IPv6 地址，应用程序可以使用 getaddrinfo() 函数解析完全限定的域名 (FQDN) name.prnp.net，其中名称是正在解析的对等名称。 pnrp.net 域是 Windows Vista 中保留用于 PNRP 名称解析的域。  
  
 PeerToPeer 应用程序之间传递的消息仍由基础体系结构处理，例如 PeerChannel 和 WCF [大型数据和流](../wcf/feature-details/large-data-and-streaming.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.PeerToPeer>
