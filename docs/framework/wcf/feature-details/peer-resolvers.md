---
title: 对等解析程序
ms.date: 03/30/2017
ms.assetid: d86d12a1-7358-450f-9727-b6afb95adb9c
ms.openlocfilehash: c9fcfbd416fb530898b2bd963dd0df1e5c1bfd62
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272106"
---
# <a name="peer-resolvers"></a>对等解析程序

为了连接到网格，对等节点需要获得其他节点的 IP 地址。 IP 地址可通过联系解析程序服务来获取，该服务获取网格 ID，然后返回与向该特定网格 ID 注册的节点对应的地址列表。 解析程序保持已注册地址的列表，此列表是它通过让网格中的每个节点向该服务注册而创建的。  
  
 可以通过 `Resolver` 的 <xref:System.ServiceModel.NetPeerTcpBinding> 属性指定使用哪个 PeerResolver 服务。  
  
## <a name="supported-peer-resolvers"></a>支持的对等解析程序  

 对等通道支持两种类型的解析程序：对等名称解析协议 (PNRP) 和自定义解析程序服务。  
  
 默认情况下，对等通道使用 PNRP 对等解析程序服务发现网格中的对等方和邻居。 对于 PNRP 不可用或不可行的情况/平台，Windows Communication Foundation (WCF) 提供一种基于服务器的替代发现服务，即 <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> 。 也可以通过编写实现 <xref:System.ServiceModel.PeerResolvers.IPeerResolverContract> 接口的类来显式定义自定义解析程序服务。  
  
### <a name="peer-name-resolution-protocol-pnrp"></a>对等名称解析协议 (PNRP)  

 PNRP （适用于 Windows Vista 的默认冲突解决程序）是一个分布式的无服务器名称解析程序服务。 还可以通过安装高级网络包，在 Windows XP SP2 上使用 PNRP。 只要满足一定条件（例如中间不存在企业防火墙），则任何两个运行相同版本 PNRP 的客户端都可以使用此协议找到对方。 请注意，Windows Vista 附带的 PNRP 版本比高级网络包中包含的版本更新。 在 Microsoft 下载中心查看适用于 Windows XP SP2 的 PNRP 更新。  
  
### <a name="custom-resolver-services"></a>自定义解析程序服务  

 当 PNRP 服务不可用或您希望完全控制网格形状时，可以使用自定义的基于服务器的解析程序服务。 可以通过编写一个实现 <xref:System.ServiceModel.PeerResolvers.IPeerResolverContract> 接口的解析程序类，或通过使用系统附带的默认实现 <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> 来显式定义此服务。  
  
 按照该服务的默认实现，如果客户端不显式刷新注册，则在经过一定的时间后，客户端注册将到期。 为了能及时地成功刷新注册，使用该解析程序服务的客户端必须了解客户端-服务器延迟的上限。 这需要在解析程序服务中选择适当的刷新超时 (`RefreshInterval`)。  (有关详细信息，请参阅 [CustomPeerResolverService：客户端注册中的](inside-the-custompeerresolverservice-client-registrations.md)。 )   
  
 应用程序编写者还必须考虑保护客户端和自定义解析程序服务之间连接的安全。 使用客户端用来联系解析程序服务的 <xref:System.ServiceModel.NetTcpBinding> 上的安全设置，可以达到此目的。 在用于创建对等通道的 `ChannelFactory` 上，必须指定凭据（如果使用）。 这些凭据将被传递到用来创建通向自定义解析程序的通道的 `ChannelFactory`。  
  
> [!NOTE]
> 当在本地网络和临时网络中使用自定义解析程序时，强烈建议使用或支持链接本地网络或临时网络的应用程序中包含相应的逻辑，以便选择要在连接时使用的单个链接本地地址。 这可以防止具有多个链接本地地址的计算机产生任何混淆。 有鉴于此，对等通道仅支持在任一时刻使用单个链接本地地址。 可以使用 `ListenIpAddress` 上的 <xref:System.ServiceModel.NetPeerTcpBinding> 属性来指定该地址。  
  
 有关如何实现自定义冲突解决程序的演示，请参阅 [对等通道自定义对等解析程序](/previous-versions/dotnet/netframework-3.5/ms751466(v=vs.90))。  
  
## <a name="in-this-section"></a>本节内容  

 [CustomPeerResolverService 内部：客户端注册](inside-the-custompeerresolverservice-client-registrations.md)  
  
## <a name="see-also"></a>另请参阅

- [对等通道概念](peer-channel-concepts.md)
- [对等通道安全性](peer-channel-security.md)
- [生成对等通道应用程序](building-a-peer-channel-application.md)
