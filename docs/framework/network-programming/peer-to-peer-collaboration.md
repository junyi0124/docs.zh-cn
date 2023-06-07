---
title: 对等协作
ms.date: 03/30/2017
ms.assetid: b6216d88-bccb-4a59-9f1c-9f751708e811
ms.openlocfilehash: 7cf92f6bf3c269e584cb8b3cdcf910be5b89fd7e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047387"
---
# <a name="peer-to-peer-collaboration"></a>对等协作

对等网络服务利用存在于 Internet 边缘的相对强大的计算机（个人计算机）完成各项任务，不仅仅是基于客户端的计算任务。 现代个人电脑 (PC) 具有超快处理器、大量存储和大型硬盘，在执行电子邮件和 Web 浏览等常见计算任务时，并未充分利用这些优势。 现代 PC 可以轻松地充当许多类型的应用程序的客户端和服务器（对等机）。  
  
对等协作基础架构是 Microsoft Windows 对等基础架构的简化实现，它利用了 Windows Vista 及更高版本平台中的网络邻居服务。 它最适用于网络邻居服务运行的子网内启用了对等的应用程序，尽管它也可为 Internet 终结点或联系人提供服务。 它集成了 Live Messenger 和其他 Live 感知应用程序使用的常见联系人管理器，以确定联系人终结点、可用性和状态。  
  
## <a name="collaboration-applications"></a>协作应用程序

 典型的对等协作应用程序包含以下步骤：  
  
- 对等机确定有兴趣托管协作会话的对等机身份  
  
- 以某种方式发送托管会话请求，主机对等机同意管理协作活动。  
  
- 主机邀请子网联系人（包括请求者）参与会话。  
  
- 希望协作的所有对等机均可将主机添加到其联系人管理器。  
  
- 无论接受还是拒绝，大多数对等机将及时向主机对等机发回邀请回复。  
  
- 所有希望协作的对等机都将订阅主机对等机。  
  
- 当对等机正在执行其初始协作活动时，主机对等机可向其联系人管理器添加远程对等机。 它还处理所有邀请回复，以确定接受者、拒绝者、未答复者。  它可能会取消邀请那些尚未答复或执行一些其他活动的人。  
  
- 此时，主机对等机可与所有受邀对等机开始协作会话，或向协作基础架构注册应用程序。  P2P 应用程序使用对等协作基础架构和 <xref:System.Net.PeerToPeer.Collaboration> 命名空间来协调游戏、公告牌、会议和其他无服务器状态应用程序的通信。  
  
## <a name="peer-to-peer-networking-security"></a>对等网络安全  

 在 Active Directory 域中，域控制器使用 Kerberos 提供身份验证服务。 在无服务器对等环境中，对等机必须提供其自身的身份验证。 对于对等网络，任何节点都可充当 CA，无需要求每个对等机受信任根存储中存在根证书。 使用自签名证书提供身份验证，格式为 X.509 证书。 这些是由每个对等机创建的证书，其生成公钥/私钥对以及使用私钥签名的证书。 自签名证书用于身份验证，并提供有关对等机实体的信息。 如 X.509 身份验证一样，对等机网络身份验证依赖于一系列追溯受信任公钥的证书。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.PeerToPeer.Collaboration>
- [关于 System.Net.PeerToPeer.Collaboration 命名空间](about-the-system-net-peertopeer-collaboration-namespace.md)
