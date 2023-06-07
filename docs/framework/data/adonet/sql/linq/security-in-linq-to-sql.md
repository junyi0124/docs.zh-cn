---
title: LINQ to SQL 中的安全性
ms.date: 03/30/2017
ms.assetid: d49787f7-414e-4c71-aa33-80a5895536b1
ms.openlocfilehash: 6260f0c565a25764c8fabd2770d4f06a987aa9bb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173388"
---
# <a name="security-in-linq-to-sql"></a>LINQ to SQL 中的安全性

连接到数据库时始终都存在安全风险。 尽管 LINQ to SQL 可能包括一些使用 SQL Server 中的数据的新方法，但它并没有提供任何附加安全机制。  
  
## <a name="access-control-and-authentication"></a>访问控制和身份验证  

 LINQ to SQL 没有自己的用户模型或身份验证机制。 使用 SQL Server 安全性可控制对映射到对象模型的数据库、数据库表、视图和存储过程的访问。 授予用户所需的最低访问权限并要求对用户身份验证使用强密码。  
  
## <a name="mapping-and-schema-information"></a>映射和架构信息  

 对象模型或外部映射文件中的 SQL-CLR 类型映射和数据库架构信息可供访问文件系统中的这些文件的所有用户使用。 假设架构信息将可供所有可以访问对象模型或外部映射文件的用户使用。 若要防止更广泛的架构信息访问，请使用文件安全机制来保护源文件和映射文件。  
  
## <a name="connection-strings"></a>连接字符串  

 应尽可能避免在连接字符串中使用密码。 连接字符串不仅会在自己的权限方面带来安全风险，而且在使用对象关系设计器或 SQLMetal 命令行工具时，连接字符串还可能会以明文形式添加到对象模型或外部映射文件。 可通过文件系统访问对象模型或外部映射文件的任何用户都可以查看连接密码（如果该密码包含在连接字符串中）。  
  
 若要最大程度地减少此类风险，请使用集成安全性与 SQL Server 进行可信连接。 通过使用这种方法，您无需将密码存储在连接字符串中。 有关详细信息，请参阅 [SQL Server 安全性](../sql-server-security.md)。  
  
 如果缺少集成安全性，则连接字符串中将需要明文密码。 帮助保护连接字符串的最佳方法如下（按风险升序排列）：  
  
- 使用集成安全性。  
  
- 通过密码保护连接字符串并尽可能减少连接字符串的传递。  
  
- 使用 <xref:System.Data.SqlClient.SqlConnection?displayProperty=nameWithType> 类来替代连接字符串，因为它可以限制公开的持续时间。 可使用 <xref:System.Data.Linq.DataContext?displayProperty=nameWithType> 来实例化 LINQ to SQL <xref:System.Data.SqlClient.SqlConnection> 类。  
  
- 最大程度地减少所有连接字符串的生存期和接触点。  
  
## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
- [常见问题解答](frequently-asked-questions.md)
