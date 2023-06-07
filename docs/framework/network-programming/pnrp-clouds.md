---
title: PNRP 云
ms.date: 03/30/2017
ms.assetid: a82e2bf1-62ab-4c2d-83f3-3217a6aead2e
ms.openlocfilehash: 60b6fb44116fe2d8af50fb0b310615b3b962977b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263199"
---
# <a name="pnrp-clouds"></a>PNRP 云

PNRP“云”表示通过网络相互通信的一组节点。 术语“云”与“对等网格”和“对等图”同义。  
  
 节点之间的通信绝不能从一个云跨越至另一个云。 <xref:System.Net.PeerToPeer.Cloud> 实例由其名称（区分大小写）唯一标识。 单个对等机或节点可以连接到多个云。  
  
 云与网络接口紧密相关。  在有两个网卡附加到不同子网的一个多宿主计算机上，将返回三个云：每个接口的每个链接本地地址有一个云，还有一个全局范围云。  
  
 PNRP 使用三个云“范围”，其中范围指一组可互相找到的计算机：  
  
- 全局云对应于全局 IPv6 地址范围和全局地址，表示整个 IPv6 Internet 上的所有计算机。 全局云只有一个。  
  
- 链接-本地云对应于链接-本地 IPv6 地址范围和链接-本地地址。 链接-本地云针对特定链接，通常与本地附加的子网相同。 链接-本地云可以有多个。  
  
 第三种云是站点特定的云，对应于站点 IPv6 地址范围和站点-本地地址。 这类云已被弃用，尽管它仍受 PNRP 支持。  
  
## <a name="clouds"></a>云  

 PNRP 云由 <xref:System.Net.PeerToPeer.Cloud> 类的实例表示。 对等机使用的多组云由可枚举的 <xref:System.Net.PeerToPeer.CloudCollection> 类的实例表示。 通过调用静态 <xref:System.Net.PeerToPeer.Cloud.GetAvailableClouds%2A> 方法，可以获取当前对等机已知的 PNRP 云的集合。  
  
 各个云具有唯一的名称，表示为 256 个字符的 Unicode 字符串。 这些名称以及上面提到的范围共同用于构造 Cloud 类的唯一实例。 可以将这些实例进行序列化和重新构造，以便永久使用。  
  
 创建或获取 Cloud 实例后，便可为其注册对等名称以创建已知对等网格。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.PeerToPeer.Cloud>
- [对等名称解析协议](peer-name-resolution-protocol.md)
