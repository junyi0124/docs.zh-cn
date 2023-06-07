---
title: 上下文连接
ms.date: 03/30/2017
ms.assetid: e443ca86-9243-4234-a822-ed10a53a9de0
ms.openlocfilehash: 85b303d62b619a8139ca56b23cacd3411cebc17b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91188845"
---
# <a name="the-context-connection"></a>上下文连接

内部数据访问问题是一种非常常见的情况。 即您希望访问正在执行公共语言运行时 (CLR) 存储过程或函数的相同服务器。 一种选择是使用 <xref:System.Data.SqlClient.SqlConnection> 创建连接，指定指向本地服务器的连接字符串，然后打开该连接。 这要求指定登录凭据。 连接与存储过程或函数处于不同的数据库会话中，它可能具有不同的 `SET` 选项，位于单独的事务中，找不到临时表等等。 如果托管存储过程或函数代码正在 SQL Server 进程中执行，则是因为有人连接到了该服务器并执行了 SQL 语句调用它。 您可能希望在该连接上下文中执行存储过程或函数，以及其事务、`SET` 选项等。 这称为上下文连接。  
  
 利用上下文连接，您可以在首先调用代码的同一上下文中执行 Transact-SQL 语句。 有关更多详细信息，请参见您正在使用的 SQL Server 版本的“SQL Server 联机丛书”版本。  
  
 **SQL Server 文档**  
  
1. [上下文连接](/sql/relational-databases/clr-integration/data-access/context-connection)  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
