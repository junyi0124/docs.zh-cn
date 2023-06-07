---
title: IPv6 寻址
description: 了解 Internet 协议版本 6 (IPv6) 地址，包括文本表示形式和地址类型。
ms.date: 03/30/2017
helpviewer_keywords:
- Internet Protocol version 6, addresses in
- Neighbor Discovery
- IPv6, enabling
- IPv6, mobility and
- Internet Protocol version 6, anycast addresses
- IPv6, Neighbor Discovery
- Internet Protocol version 6, auto-configuring
- Internet Protocol version 6, enabling
- IPv6, unicast addresses
- IPv6, auto-configuring
- Internet Protocol version 6, routing
- IPv6, RFC documents
- IPv6, routing
- Internet Protocol version 6, disabling
- Internet Protocol version 6, unicast addresses
- IPv6, multicast addresses
- Internet Protocol version 6, mobility and
- Internet Protocol version 6, RFC documents
- Internet Protocol version 6, Neighbor Discovery
- IPv6, anycast addresses
- Internet Protocol version 6, multicast addresses
- IPv6, addresses in
- IPv6, disabling
ms.assetid: 20a104ae-1649-4649-a005-531a5cf74c93
ms.openlocfilehash: fbf68cb5f40450c2f9ecf4900801ee55e326fcb4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502335"
---
# <a name="ipv6-addressing"></a>IPv6 寻址

在 Internet 协议版本 6（IPv6）中，地址长度为 128 位。 地址空间如此之大的一个原因是将可用地址细分为可以反映 Internet 拓扑的路由域的层次结构。 另一个原因是映射将设备连接到网络的网络适配器（或接口）的地址。 IPv6 有可以解析最低级别的地址（即网络接口级别的地址）的固有功能以及自动配置功能。

## <a name="text-representation"></a>文本表示形式

以下是用于将 IPv6 地址表示为文本字符串的三种常规形式：

- 冒号十六进制形式。 这是首选形式 n:n:n:n:n:n:n:n。 每个 n 表示地址的 8 个 16 位元素之一的十六进制值。 例如：`3FFE:FFFF:7654:FEDA:1245:BA98:3210:4562`。

- 压缩形式。 由于地址长度，有些地址通常包含一长串的零。 若要简化这些地址的编写，可使用压缩形式，其中 0 块的单个相邻的序列由双冒号 (::) 表示。 该符号在地址中只能出现一次。 例如，多播地址 `FFED:0:0:0:0:BA98:3210:4562` 的压缩形式是 `FFED::BA98:3210:4562`。 单播地址 `3FFE:FFFF:0:0:8:800:20C4:0` 的压缩形式是 `3FFE:FFFF::8:800:20C4:0`。 环回地址 `0:0:0:0:0:0:0:1` 的压缩形式是 `::` 1。 未指定地址 `0:0:0:0:0:0:0:0` 的压缩形式是 `::`。

- **混合形式**。 此形式合并了 IPv4 和 IPv6 地址。 在这种情况下，地址格式为 n:n:n:n:n:n:d.d.d.d，其中每个 n 表示六个 IPv6 高序位 16 位地址元素的十六进制值，每个 d 表示一个 IPv4 地址的十进制值。

## <a name="address-types"></a>地址类型

地址中的前导位定义了特定 IPv6 地址类型。 包含这些前导位的可变长度字段称为格式前缀 (FP)。

IPv6 的单播地址分为两个部分。 第一部分包含地址前缀，第二部分包含接口标识符。 表示 IPv6 地址/前缀组合的简洁方法如下：ipv6-address/prefix-length。

以下是具有 64 位前缀的地址示例。

`3FFE:FFFF:0:CD30:0:0:0:0/64`。

本示例中的前缀是 `3FFE:FFFF:0:CD30`。 此地址也可以采用压缩形式编写为 `3FFE:FFFF:0:CD30::/64`。

IPv6 定义以下地址类型：

- 单播地址。 单个接口的标识符。 发送到此地址的数据包将会发送到已标识接口。 可通过高序位八进制数的值，区分单播地址和多播地址。 多播地址的高序位八进制数具有 FF 的十六进制值。 此八进制数的任何其他值标识单播地址。 以下是不同类型的单播地址：

  - 链接本地地址。 这些地址用于单个链接，并且具有以下格式：FE80::InterfaceID。 链接本地地址用于链接的节点之间以实现自动地址配置、邻居发现，或者用于不存在路由器的情况。 链接本地地址主要用于启动时，以及系统尚未获取较大范围地址时。

  - 站点本地地址。 这些地址用于单个站点，具有以下格式：FEC0::SubnetID:InterfaceID 。 站点本地地址用于在站点内寻址，无需全局前缀。

  - 全局 IPv6 单播地址。 这些地址可以在 Internet 上使用，并具有以下格式：010 (FP，3 位) TLA ID (13 位)预留(8 位) NLA ID (24 位) SLA ID (16 位) InterfaceID (64 位)

- 多播地址。 一组接口（通常属于不同节点）的标识符。 发送到此地址的数据包将发送到由该地址标识的所有接口。 多播地址类型取代了 IPv4 广播地址。

- 任播地址。 一组接口（通常属于不同节点）的标识符。 发送到此地址的数据包将发送到由该地址标识的仅一个接口。 这是路由指标标识的最近接口。 任播地址取自单播地址空间，无法从语法上进行区分。 寻址的接口根据其配置区分单播和任播地址。

通常，节点始终具有链接本地地址。 它可能具有站点本地地址和一个或多个全局地址。

## <a name="see-also"></a>请参阅

- [Internet 协议版本 6](internet-protocol-version-6.md)
- [套接字](sockets.md)
