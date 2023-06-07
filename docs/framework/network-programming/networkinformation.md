---
title: NetworkInformation
ms.date: 03/30/2017
helpviewer_keywords:
- Network
ms.assetid: 31b44dd3-b903-4a48-8419-40419a3e4038
ms.openlocfilehash: 550555be7cc95cafebabfd3ac230ced024a9202c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261613"
---
# <a name="networkinformation"></a>NetworkInformation

<xref:System.Net.NetworkInformation> 命名空间使你能够收集有关网络事件、更改、统计信息和属性的信息。 还可使用 <xref:System.Net.NetworkInformation.Ping?displayProperty=nameWithType> 类来确定是否可以访问远程主机。  
  
## <a name="network-availability-and-events"></a>网络可用性和事件  

 <xref:System.Net.NetworkInformation.NetworkChange?displayProperty=nameWithType> 类使你能确定是否已更改网络地址或可用性。 若要使用此类，请创建事件处理程序来处理更改，并将其与 <xref:System.Net.NetworkInformation.NetworkAddressChangedEventHandler> 或 <xref:System.Net.NetworkInformation.NetworkAvailabilityChangedEventHandler> 关联。 有关详细信息，请参阅[如何：检测网络可用性和地址更改](how-to-detect-network-availability-and-address-changes.md)。  
  
## <a name="network-statistics-and-properties"></a>网络统计信息和属性  

 可在接口或协议基础上收集网络统计信息和属性。 <xref:System.Net.NetworkInformation.NetworkInterface>、<xref:System.Net.NetworkInformation.NetworkInterfaceType> 和 <xref:System.Net.NetworkInformation.PhysicalAddress> 类提供有关特定网络接口的信息，而 <xref:System.Net.NetworkInformation.IPInterfaceProperties>、<xref:System.Net.NetworkInformation.IPGlobalProperties>、<xref:System.Net.NetworkInformation.IPGlobalStatistics>、<xref:System.Net.NetworkInformation.TcpStatistics> 和 <xref:System.Net.NetworkInformation.UdpStatistics> 类提供有关第 3 层和第 4 层数据包的信息。 有关详细信息，请参阅[如何：获取接口和协议信息](how-to-get-interface-and-protocol-information.md)。  
  
## <a name="determine-if-a-remote-host-is-reachable"></a>确定是否可访问远程主机  

 可使用 <xref:System.Net.NetworkInformation.Ping> 类来确定远程主机是否运行、是否在线及是否可访问。 有关详细信息，请参阅[如何：Ping 主机](how-to-ping-a-host.md)。  
  
## <a name="see-also"></a>另请参阅

- [网络编程示例](network-programming-samples.md)

<!-- to-do: review sample links
- [Network Information Technology Sample](https://archive.msdn.microsoft.com/nclsamples/Wiki/View.aspx?title=Network%20Information)
- [NetStat Tool Technology Sample](https://archive.msdn.microsoft.com/nclsamples/Wiki/View.aspx?title=NetStat%20Tool)
- [Ping Client Technology Sample](https://archive.msdn.microsoft.com/nclsamples/Wiki/View.aspx?title=Ping%20Client)
-->
