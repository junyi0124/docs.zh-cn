---
title: 启用多个活动结果集
description: 了解如何在连接字符串中启用/禁用 MARS，这与 SQL Server 一起使用，以便可以在 ADO.NET 中的单个连接上运行多个批处理。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 576079e4-debe-4ab5-9204-fcbe2ca7a5e2
ms.openlocfilehash: 0c5b4043b389c7dde39a477f90e82bbf654331f7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156246"
---
# <a name="enabling-multiple-active-result-sets"></a>启用多个活动结果集

多重活动结果集 (MARS) 是一项用于 SQL Server 的功能，可用来对单个连接执行多个批处理。 如果对 SQL Server 启用了 MARS，使用的每个命令对象都将向该连接添加一个会话。  
  
> [!NOTE]
> 单个 MARS 会话打开一个供 MARS 使用的逻辑连接，然后为每个活动命令打开一个逻辑连接。  
  
## <a name="enabling-and-disabling-mars-in-the-connection-string"></a>在连接字符串中启用和禁用 MARS  
  
> [!NOTE]
> 下列连接字符串使用随 SQL Server 提供的 AdventureWorks 示例数据库****。 提供的连接字符串假定数据库安装在名为 MSSQL1 的服务器上。 根据环境需要修改连接字符串。  
  
 默认情况下，MARS 功能处于禁用状态。 可以通过将“MultipleActiveResultSets=True”关键字对添加到连接字符串来启用它。 “True”是启用 MARS 的唯一有效值。 下面的示例演示如何连接到 SQL Server 的实例以及如何指定是否应启用 MARS。  
  
```vb  
Dim connectionString As String = "Data Source=MSSQL1;" & _  
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" & _  
    "MultipleActiveResultSets=True"  
```  
  
```csharp  
string connectionString = "Data Source=MSSQL1;" +
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" +  
    "MultipleActiveResultSets=True";  
```  
  
 可以通过将“MultipleActiveResultSets=False”关键字对添加到连接字符串来禁用 MARS。 “False”是禁用 MARS 的唯一有效值。 以下连接字符串演示了如何禁用 MARS。  
  
```vb  
Dim connectionString As String = "Data Source=MSSQL1;" & _  
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" & _  
    "MultipleActiveResultSets=False"  
```  
  
```csharp  
string connectionString = "Data Source=MSSQL1;" +
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" +  
    "MultipleActiveResultSets=False";  
```  
  
## <a name="special-considerations-when-using-mars"></a>使用 MARS 时的特殊注意事项  

 通常，现有应用程序无需修改即可使用启用 MARS 的连接。 但是，如果希望在应用程序中使用 MARS 功能，则应了解以下特殊注意事项。  
  
### <a name="statement-interleaving"></a>语句交替  

 MARS 操作在服务器上同步执行。 允许 SELECT 和 BULK INSERT 语句的语句交错。 但是，数据操作语言 (DML) 和数据定义语言 (DDL) 语句是以原子方式执行的。 在执行原子批处理时，无法尝试执行任何语句。 服务器上的并发执行不是 MARS 功能。  
  
 如果在 MARS 连接下提交了两个批处理，其中一个包含 SELECT 语句，另一个包含 DML 语句，则可以在 SELECT 语句执行期间开始执行 DML。 但是，DML 语句必须先运行到完成，SELECT 语句才能取得进展。 如果两个语句都在同一事务下运行，那么在 SELECT 语句开始执行之后 DML 语句所做的任何更改对于读取操作都是不可见的。  
  
 SELECT 语句中的 WAITFOR 语句在等待时（即直到生成第一行之前）不会产生事务。 这意味着在等待 WAITFOR 语句时，其他批处理都无法在同一连接内执行。  
  
### <a name="mars-session-cache"></a>MARS 会话缓存  

 在启用 MARS 的情况下打开连接时，将创建逻辑会话，这会增加额外的开销。 为了将系统开销降至最低并提高性能，SqlClient 将 MARS 会话缓存在连接内****。 缓存最多包含 10 个 MARS 会话。 该值不是用户可调节的。 如果达到会话限制，则会创建一个新会话，而不会生成错误。 其中包含的缓存和会话是按连接进行的；它们不会在连接之间共享。 发布会话时，会将其返回到池中，除非已达到池的上限。 如果缓存池已满，则会话将关闭。 MARS 会话不会过期。 仅在处置连接对象时，才清理这些会话。 MARS 会话缓存未预加载。 已加载它，因为应用程序需要更多会话。  
  
### <a name="thread-safety"></a>线程安全性  

 MARS 操作不是线程安全的。  
  
### <a name="connection-pooling"></a>连接池  

 启用了 MARS 的连接像其他任何连接一样共用。 如果应用程序打开两个连接，一个启用了 MARS，一个禁用了 MARS，则两个连接位于不同的池中。 有关详细信息，请参阅 [SQL Server 连接池 (ADO.NET)](../sql-server-connection-pooling.md)。  
  
### <a name="sql-server-batch-execution-environment"></a>SQL Server 批处理执行环境  

 打开连接时，将定义默认环境。 然后，将此环境复制到逻辑 MARS 会话中。  
  
 批处理执行环境包括以下组件：  
  
- Set 选项（例如，ANSI_NULLS、DATE_FORMAT、LANGUAGE、TEXTSIZE）  
  
- 安全性上下文（用户/应用程序角色）  
  
- 数据库上下文（当前数据库）  
  
- 执行状态变量（例如，@@ERROR、@@ROWCOUNT、@@FETCH_STATUS、@@IDENTITY）  
  
- 顶级临时表  
  
 借助 MARS，将默认执行环境与连接关联起来。 在特定连接下开始执行的每个新批处理都将接收默认环境的一个副本。 每当在给定的批处理下执行代码时，对环境所做的所有更改都将限制在特定的批处理范围内。 执行完成后，执行设置将复制到默认环境中。 如果一个批处理发出多个要在同一事务下按顺序执行的命令，语义将与早期客户端或服务器中涉及的连接所显示出来的语义相同。  
  
### <a name="parallel-execution"></a>并行执行  

 MARS 并非旨在删除对应用程序中多个连接的所有要求。 如果应用程序需要对服务器执行真正的并发命令，则应使用多个连接。  
  
 例如，考虑以下情况。 创建了两个命令对象，一个用于处理结果集，另一个用于更新数据；他们通过 MARS 共享公共连接。 在此方案中，`Transaction`.`Commit` 在更新时失败，直到在第一个命令对象上读取了所有结果，并生成以下异常：  
  
 消息：其他会话正在使用事务的上下文。  
  
 源： .NET SqlClient 数据提供程序  
  
 应为：(null)  
  
 收到：System.Data.SqlClient.SqlException  
  
 有两种用于处理此方案的选项：  
  
1. 创建读取器后，启动事务，以使其不属于事务。 然后，每次更新都将成为其自己的事务。  
  
2. 关闭读取器后，提交所有工作。 这可能会产生大量更新。  
  
3. 不要使用 MARS；而是像在 MARS 之前那样为每个命令对象使用单独的连接。  
  
### <a name="detecting-mars-support"></a>检测 MARS 支持  

 应用程序可以通过读取 `SqlConnection.ServerVersion` 值来检查 MARS 支持。 SQL Server 2005 的主版本号为 9；SQL Server 2008 的主版本号为 10。  
  
## <a name="see-also"></a>请参阅

- [多重活动结果集 (MARS)](multiple-active-result-sets-mars.md)
- [ADO.NET 概述](../ado-net-overview.md)
