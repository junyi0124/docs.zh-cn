---
title: 对等名称和 PNRP ID
ms.date: 03/30/2017
ms.assetid: afa538e8-948f-4a98-aa9f-305134004115
ms.openlocfilehash: a56200d599d417dac549af000b35024338d43796
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239219"
---
# <a name="peer-names-and-pnrp-ids"></a>对等名称和 PNRP ID

对等名称代表通信的终结点，可以是计算机、用户、组、服务或者任何与对等有关的、可以解析为 IPv6 地址的事物。 对等名称解析协议 (PNRP) 将统计学上唯一的对等名称用于创建标识云成员的 PNRP ID。  
  
## <a name="peer-names"></a>对等名称  

 对等名称可以注册成不安全的或安全的。 不安全名称是文本字符串，存在遭受欺骗的风险，因为任何人都可注册重复的不安全名称。 不安全名称最适用于专用或受保护的网络。 安全名称受到证书和数字签名的保护。 只有原发布者才能证实自己拥有安全名称。  
  
 云和范围的组合为参与 PNRP 活动的对等方提供了相当安全的环境。 但使用安全对等名称并不能确保网络应用程序的整体安全性。 应用程序的安全性是视具体实现而定的。  
  
 安全对等名称仅由其所有者注册，并使用公钥加密进行保护。 安全对等名称被视为属于拥有相应私钥的对等实体。 可以通过使用私钥签名的认证对等地址 (CPA) 验证所有权。 恶意用户没有相应的私钥，无法伪造对等名称的所有权。  
  
## <a name="pnrp-ids"></a>PNRP ID  

 ![PNRP ID](./media/fdc9e8a0-4a1c-488d-a019-bc3a1973220c.gif "fdc9e8a0-4a1c-488d-a019-bc3a1973220c")  
  
 PNRP ID 由以下内容组成：  
  
- 高序位 128 位，也就是对等 (P2P) ID，是分配至终结点的对等名称的哈希代码。 对等名称采用以下格式：*Authority.Classifier*。 对于安全名称，Authority 是对等名称公钥的安全哈希算法 1 (SHA1) 的哈希代码，是十六进制字符。 对于不安全名称，Authority 为单字符“0”。  是标识应用程序的字符串。 对等名称 Classifier 最多长 149 个字符，包括 `null` 终止符在内。  
  
- 低序位 128 位用于服务定位，它是标识同一云中同一 P2P ID 的不同实例的生成数字。  
  
 P2P ID 和服务定位的这种组合实现从一台计算机注册多个 PNRP ID。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.PeerToPeer.PeerName>
- <xref:System.Net.PeerToPeer>
