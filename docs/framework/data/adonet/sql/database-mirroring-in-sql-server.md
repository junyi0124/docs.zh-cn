---
title: SQL Server 中的数据库镜像
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 89befaff-bb46-4290-8382-e67cdb0e3de9
ms.openlocfilehash: 7e2c1c8ea1cbc1bb22452b9ef9d1f250c96118ea
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173531"
---
# <a name="database-mirroring-in-sql-server"></a>SQL Server 中的数据库镜像

通过 SQL Server 中的数据库镜像，可以在备用服务器上保留 SQL Server 数据库的副本（即镜像）。 镜像可确保始终存在数据的两个独立副本，从而提供高可用性和完整的数据冗余。 用于 SQL Server 的 .NET 数据提供程序提供了数据库镜像的隐式支持，这样，在为 SQL Server 数据库配置了镜像之后，开发人员无需采取任何措施或编写任何代码。 此外，<xref:System.Data.SqlClient.SqlConnection> 对象还支持显式连接模式，即允许在 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> 中提供故障转移伙伴服务器的名称。  
  
 下面是定目标到已配置镜像的数据库的 <xref:System.Data.SqlClient.SqlConnection> 对象的简化事件序列：  
  
1. 客户端应用程序成功连接到主体数据库，服务器发送回伙伴服务器的名称，然后将它缓存在客户端上。  
  
2. 如果包含主体数据库的服务器发生故障或连接中断，连接和事务状态会丢失。 客户端应用程序尝试重新与主体数据库建立连接，但失败了。  
  
3. 然后，客户端应用程序以透明方式尝试与伙伴服务器上的镜像数据库建立连接。 如果成功，连接会重定向到镜像数据库，然后镜像数据库会成为新的主体数据库。  
  
## <a name="specifying-the-failover-partner-in-the-connection-string"></a>在连接字符串中指定故障转移合作伙伴  

 如果你在连接字符串中提供故障转移伙伴服务器的名称，且主体数据库在客户端应用程序首次连接时不可用，那么客户端会以透明方式尝试与故障转移伙伴建立连接。  
  
```csharp
";Failover Partner=PartnerServerName"  
```  
  
 如果你省略故障转移伙伴服务器的名称，且主体数据库在客户端应用程序首次连接时不可用，那么 <xref:System.Data.SqlClient.SqlException> 会抛出。  
  
 当 <xref:System.Data.SqlClient.SqlConnection> 成功打开时，服务器会返回故障转移伙伴名称，此名称取代连接字符串中提供的任何值。  
  
> [!NOTE]
> 对于数据库镜像方案，必须在连接字符串中显式指定初始目录或数据库名称。 如果客户端接收没有显式指定初始编录或数据库的连接的故障转移信息，故障转移信息不会缓存，应用程序也不会在主体服务器失败时尝试进行故障转移。 如果连接字符串有故障转移伙伴的值，但没有初始目录或数据库的值，那么 `InvalidArgumentException` 会抛出。  
  
## <a name="retrieving-the-current-server-name"></a>检索当前服务器名称  

 在发生故障转移时，可以使用 <xref:System.Data.SqlClient.SqlConnection> 对象的 <xref:System.Data.SqlClient.SqlConnection.DataSource%2A> 属性，检索当前连接实际连接到的服务器的名称。 下面的代码片段检索活动服务器的名称（假设连接变量引用打开的 <xref:System.Data.SqlClient.SqlConnection>）。  
  
 当故障转移事件发生且连接切换到镜像服务器时，DataSource  属性会更新，以反映镜像名称。  
  
```vb  
Dim activeServer As String = connection.DataSource  
```  
  
```csharp  
string activeServer = connection.DataSource;  
```  
  
## <a name="sqlclient-mirroring-behavior"></a>SqlClient 镜像行为  

 客户端始终尝试连接到当前主体服务器。 如果失败，它会尝试连接到故障转移伙伴。 如果镜像数据库已切换为伙伴服务器上的主体角色，连接会成功，且新的主体镜像映射会发送到客户端，并在调用 <xref:System.AppDomain> 的生存期内进行缓存。 映射不会存储在永久存储空间中，也无法供其他 AppDomain 或进程中的后续连接使用  。 但是，可以供相同 AppDomain 中的后续连接使用  。 注意，在相同或不同的计算机上运行的其他 AppDomain 或进程总是拥有自己的连接池，这些连接不会重置  。 在这种情况下，如果主数据库关闭，每个进程或 AppDomain 将失败，池将被自动清除  。  
  
> [!NOTE]
> 服务器上的镜像支持是逐个数据库进行配置的。 如果通过使用多部分名称或更改当前数据库对主体/镜像集中没有的其他数据库执行数据操作，对这些其他数据库所做的更改不会在发生故障时传播。 修改未镜像的数据库中的数据不会生成错误。 开发人员必须评估此类操作可能会造成的影响。  
  
## <a name="database-mirroring-resources"></a>数据库镜像资源  

 有关配置、部署和管理镜像的概念性文档及信息，请参阅 SQL Server 文档中的下列资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[数据库镜像](/sql/database-engine/database-mirroring/database-mirroring-sql-server)|介绍在 SQL Server 中设置和配置镜像的方法。|  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
