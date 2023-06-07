---
title: Internet 协议版本 6
description: 了解 IPv6 协议及其与 IPv4 的差异。 .NET Framework 应用程序支持 IPv6，但可能需要进行配置。
ms.date: 03/30/2017
helpviewer_keywords:
- IPv6, improvements
- IPv4
- IPv6
- Internet Protocol version 6, improvements
- Internet Protocol version 6
ms.assetid: e6fa8ebd-010a-4c48-a5ec-a5102c53c06f
ms.openlocfilehash: f5b74674ba4144f75d125267f2458394bb74fab1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283947"
---
# <a name="internet-protocol-version-6"></a>Internet 协议版本 6

Internet 协议版本 6 (IPv6) 是 Internet 的网络层的标准协议新套件。 IPv6 旨在解决当前版本的 Internet 协议套件（称作 IPv4）存在的许多问题，包括地址消耗、安全性、自动配置和扩展性等问题。 IPv6 扩展了 Internet 的功能以启用新型应用程序，包括对等和移动应用程序。 以下是当前 IPv4 协议的主要问题：  
  
- 地址空间快速消耗。  
  
     这导致使用网络地址转换器 (NAT) 将多个专用地址映射到一个公共 IP 地址。 此机制造成的主要问题是处理开销和端对端连接性的缺失。  
  
- 层次结构支持缺失。  
  
     因其固有的预定义类组织，IPv4 缺乏真正的层次结构支持。 无法通过真正地映射网络拓扑的方式构成 IP 地址。 由于存在这种重大的设计缺陷，所以需要通过大型路由表将 IPv4 数据包传送至 Internet 上的任何位置。  
  
- 复杂的网络配置。  
  
     对于 IPv4，地址必须以静态方式分配，或使用配置协议，如 DHCP。 在理想情况下，主机不需要依靠 DHCP 基础结构的管理。 主机可基于其所在的网络段自行配置。  
  
- 缺乏内置身份验证和保密性。  
  
     IPv4 不需要针对提供交换数据的身份验证或加密的机制的支持。 IPv6 在这一方面作出更改。 Internet 协议安全 (IPSec)是 IPv6 支持要求。  
  
 新协议套件必须满足以下基本需求：  
  
- 低开销的大规模路由和寻址。  
  
- 针对各种连接情况进行自动配置。  
  
- 内置身份验证和保密性。  
  
 有关详细信息，请参阅 [IPv6 寻址](ipv6-addressing.md)、[IPv6 路由](ipv6-routing.md)、[IPv6 自动配置](ipv6-auto-configuration.md)、[启用和禁用 IPv6](enabling-and-disabling-ipv6.md) 以及[如何：修改计算机配置文件以启用 IPv6 支持](how-to-modify-the-computer-configuration-file-to-enable-ipv6-support.md)。  
  
## <a name="references"></a>reference  

 以下是可以在 [Internet 工程任务组 (IETF)](https://www.ietf.org/) 找到的精选 RFC 文档：  
  
- RFC 1287，面向未来的 Internet 体系结构。  
  
- RFC 1454，下一 IP 版本的建议的比较。  
  
- RFC 2373，IP 版本 6 寻址体系结构。  
  
- RFC 2374，IPv6 可聚合全局单播地址格式。  
  
 还可以在 [IP 版本 6 (IPv6)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379498(v=ws.10)) 中找到 IPv6 相关信息。  
  
## <a name="see-also"></a>请参阅

- [IPv6 套接字示例](/previous-versions/dotnet/netframework-3.0/ms180981(v=vs.85))
- [网络编程示例](network-programming-samples.md)
- [套接字](sockets.md)
