---
title: SqlClient 对高可用性的支持，灾难恢复
description: 使用 AlwaysOn 可用性组了解 SQL Server 中的高可用性、灾难恢复的 SqlClient 应用程序支持。
ms.date: 03/30/2017
ms.assetid: 61e0b396-09d7-4e13-9711-7dcbcbd103a0
ms.openlocfilehash: 7693210b7d9387e9b58fcc95febd3df0c70b4743
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203431"
---
# <a name="sqlclient-support-for-high-availability-disaster-recovery"></a>SqlClient 对高可用性的支持，灾难恢复

本主题讨论 SqlClient 支持 (添加到 .NET Framework 4.5) ，以实现高可用性、灾难恢复--AlwaysOn 可用性组。  AlwaysOn 可用性组功能已添加到 SQL Server 2012 中。 有关 AlwaysOn 可用性组的详细信息，请参阅 SQL Server 联机丛书。  
  
 现可在连接属性中指定（高可用性、灾难恢复功能）可用性组 (AG) 的可用性组侦听程序或 SQL Server 2012 故障转移群集实例。 如果将 SqlClient 应用程序连接到具有故障转移功能的 AlwaysOn 数据库，则在故障转移后，系统会断开原始连接，并且该应用程序必须建立一个新的连接才能继续运行。  
  
 如果没有连接到可用性组侦听程序或 SQL Server 2012 故障转移群集实例，并且如果多个 IP 地址关联一个主机名，SqlClient 将按顺序循环访问与 DNS 条目关联的所有 IP 地址。 如果 DNS 服务器返回的第一个 IP 地址未绑定到任何网络接口卡 (NIC)，则上述遍历操作可能会用时较长。 连接到一个可用性组侦听程序或 SQL Server 2012 故障转移群集实例时，SqlClient 尝试并行建立所有 IP 地址的连接，如果一个连接尝试成功，驱动程序将丢弃所有挂起的连接尝试。  
  
> [!NOTE]
> 增大连接超时值和实现连接重试逻辑将增加应用程序连接到可用性组的概率。 此外，由于故障转移可能会使连接失败，因此请实现连接重试逻辑；重试失败的连接直至重新连接。  
  
 以下连接属性已添加到 .NET Framework 4.5 中的 SqlClient：  
  
- `ApplicationIntent`  
  
- `MultiSubnetFailover`  
  
 可以通过编程方式修改这些连接字符串关键字：  
  
1. <xref:System.Data.SqlClient.SqlConnectionStringBuilder.ApplicationIntent%2A>  
  
2. <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A>  

> [!NOTE]
> `MultiSubnetFailover` `true` .NET Framework 4.6.1 或更高版本不需要将设置为。
  
## <a name="connecting-with-multisubnetfailover"></a>使用 MultiSubnetFailover 进行连接  

 在连接到 SQL Server 2012 可用性组侦听器或 SQL Server 2012 故障转移群集实例时，应始终指定 `MultiSubnetFailover=True`。 `MultiSubnetFailover` 可加快 SQL Server 2012 中所有可用性组和故障转移群集实例的故障转移速度，并且将显著缩短单子网和多子网 AlwaysOn 拓扑的故障转移时间。 在多子网故障转移过程中，客户端将尝试并行进行连接。 在子网故障转移过程中，将会主动重试 TCP 连接。  
  
 `MultiSubnetFailover` 连接属性指示应用程序将部署到可用性组或 SQL Server 2012 故障转移群集实例，并且 SqlClient 将通过连接到所有的 IP 地址尝试连接到主 SQL Server 实例上的数据库。 为连接指定 `MultiSubnetFailover=True` 后，客户端将在比操作系统的默认 TCP 重传间隔更短的时间内重试 TCP 连接尝试。 这样，就可以在对 AlwaysOn 可用性组或 AlwaysOn 故障转移群集实例执行故障转移之后更快地进行重新连接，这一点同时适用于单子网和多子网可用性组和故障转移群集实例。  
  
 有关 SqlClient 中的连接字符串关键字的详细信息，请参阅 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>。  
  
 如果在连接到除可用性组侦听程序或 SQL Server 2012 故障转移群集实例之外的对象时指定 `MultiSubnetFailover=True`，可能导致性能下降，因此不支持这样做。  
  
 使用以下准则可以连接到可用性组或 SQL Server 2012 故障转移群集实例中的服务器：  
  
- 在连接到单子网或多子网时使用 `MultiSubnetFailover` 连接属性；这将改进这两者的性能。  
  
- 若要连接到某一可用性组，请在您的连接字符串中将该可用性组的可用性组侦听器指定为服务器。  
  
- 连接到配置有超过 64 个 IP 地址的 SQL Server 实例将导致连接失败。  
  
- 基于以下身份验证类型，使用 `MultiSubnetFailover` 连接属性的应用程序的行为将不会受到影响：SQL Server 身份验证、Kerberos 身份验证或 Windows 身份验证。  
  
- 延长 `Connect Timeout` 的值可调整故障转移时间，减少应用程序连接重试次数。  
  
- 不支持分布式事务。  
  
 如果只读路由不起作用，则连接到次要副本位置在以下情况下将失败：  
  
1. 如果未将辅助副本位置配置为接受连接。  
  
2. 如果应用程序使用 `ApplicationIntent=ReadWrite`（在下面论述）且将辅助副本位置配置为只读访问。  
  
 只读辅助副本不支持 <xref:System.Data.SqlClient.SqlDependency>。  
  
 如果将主副本配置为拒绝只读工作负荷且连接字符串包含 `ApplicationIntent=ReadOnly`，连接将失败。  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>升级以便使用来自数据库镜像的多子网群集  

 如果连接字符串中已存在 `MultiSubnetFailover` 和 `Failover Partner` 连接关键字，或者如果使用了 `MultiSubnetFailover=True` 和除 TCP 以外的其他协议，将出现连接错误 (<xref:System.ArgumentException>)。 如果使用 `MultiSubnetFailover` 且 SQL Server 返回一个故障转移伙伴响应指示它是数据库镜像对的一部分，也将出现错误 (<xref:System.Data.SqlClient.SqlException>)。  
  
 如果你将当前使用数据库镜像的 SqlClient 应用程序升级到多子网方案，则应删除 `Failover Partner` 连接属性并使用设置为 `True` 的 `MultiSubnetFailover` 替换它，并且还应使用可用性组侦听程序替换连接字符串中的服务器名称。 如果连接字符串使用 `Failover Partner` 和 `MultiSubnetFailover=True`，则驱动程序将生成错误。 但是，如果连接字符串使用 `Failover Partner` 和 `MultiSubnetFailover=False`（或 `ApplicationIntent=ReadWrite`），则应用程序将使用数据库镜像。  
  
 如果在 AG 的主数据库中使用数据库镜像，同时在连接到主数据库而不是可用性组侦听程序的连接字符串中使用 `MultiSubnetFailover=True`，则驱动程序将返回一条错误。  
  
## <a name="specifying-application-intent"></a>指定应用程序意向  

 如果 `ApplicationIntent=ReadOnly`，在连接到某一启用了 AlwaysOn 的数据库时，客户端将请求读取工作负荷。 服务器在连接时和在执行 USE 数据库语句的过程中将强制该意向，但仅针对启用了 AlwaysOn 的数据库。  
  
 `ApplicationIntent` 关键字不适用于早期的只读数据库。  
  
 数据库可允许或禁止目标 AlwaysOn 数据库上的读取工作负荷。 （这是通过 `PRIMARY_ROLE` 和 `SECONDARY_ROLE`Transact-SQL 语句的 `ALLOW_CONNECTIONS` 子句实现的。）  
  
 `ApplicationIntent` 关键字用于启用只读路由。  
  
## <a name="read-only-routing"></a>只读路由  

 只读路由是一项可确保数据库只读副本的可用性的功能。 启用只读路由：  
  
1. 您必须连接到某一 AlwaysOn 可用性组侦听器。  
  
2. `ApplicationIntent` 连接字符串关键字必须设置为 `ReadOnly`。  
  
3. 数据库管理员必须配置该可用性组以便启用只读路由。  
  
 使用只读路由的多个连接可能不会全部连接到相同的只读副本。 对数据库同步进行更改或对服务器的路由配置进行更改可能导致客户端连接到不同的只读副本。 若要确保所有只读请求都连接到相同的只读副本，请勿将可用性组侦听器传递到 `Data Source` 连接字符串关键字。 而是指定只读实例的名称。  
  
 只读路由所用的时间可能会长于连接到主副本的时间，因为只读路由首先连接到主副本，然后查找可用的最佳可读取辅助副本。 为此，应增加您的登录超时。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 功能和 ADO.NET](sql-server-features-and-adonet.md)
- [ADO.NET 概述](../ado-net-overview.md)
