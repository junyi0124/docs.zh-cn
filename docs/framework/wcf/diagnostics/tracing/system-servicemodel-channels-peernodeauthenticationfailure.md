---
title: System.ServiceModel.Channels.PeerNodeAuthenticationFailure
ms.date: 03/30/2017
ms.assetid: 0b50f782-ca06-4a82-aa7f-71f78ddc5177
ms.openlocfilehash: d8abfe6e34439ccf399e37c1285b7b71cebf9870
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258031"
---
# <a name="systemservicemodelchannelspeernodeauthenticationfailure"></a>System.ServiceModel.Channels.PeerNodeAuthenticationFailure

与潜在邻居的安全握手没有成功。  
  
## <a name="description"></a>描述  

 此跟踪在尝试建立安全邻居连接时发生。 凭据不充足或不正确可以导致出现此情况。  
  
 对等通道只能识别一种强标识令牌类型，即 X.509 证书，X.509 证书基于可实现的身份验证和授权类型，提供强标识模型。 对等通道还可通过使用密码为简单应用程序提供支持。 密码仅用于允许加入会话，不能用于执行消息身份验证。 这是因为对等端组共享的对称令牌难以、也不适合用于进行源身份验证。  
  
## <a name="troubleshooting"></a>疑难解答  

 确保所有邻居都具有适当的安全凭据。  
  
## <a name="see-also"></a>另请参阅

- [对等通道安全性](../../feature-details/peer-channel-security.md)
- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
