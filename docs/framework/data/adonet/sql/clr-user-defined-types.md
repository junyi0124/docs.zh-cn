---
title: CLR 用户定义类型
ms.date: 03/30/2017
ms.assetid: 9f70e0b0-3a0d-4eb1-b914-07a5d0c167c2
ms.openlocfilehash: 84d588e0c415daa7de19ea695c817f3eb24f732f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173583"
---
# <a name="clr-user-defined-types"></a>CLR 用户定义类型

Microsoft SQL Server 提供了对使用 Microsoft .NET Framework 公共语言运行时 (CLR) 实现的用户定义类型 (UDT) 的支持。 CLR 集成在 SQL Server 中，通过此机制，您可以扩展数据库的类型系统。 UDT 使用户可以扩展 SQL Server 的数据类型系统，还可以定义复杂的结构化类型。  
  
 从应用程序结构的角度来说，UDT 具有两个重要的优点：  
  
- 在内部状态和外部行为之间强大的包装（无论在客户端中还是在服务器中）。  
  
- 与其他相关服务器功能的深度集成。 定义了自己的 UDT 后，可以在所有可使用 SQL Server 中的系统类型（包括列定义）的上下文中使用，并且可以作为变量、参数、函数结果、游标、触发器和复制使用。  
  
 有关更多详细信息，请参阅所使用 SQL Server 版本的 [SQL Server 文档](/sql) 。
  
 **SQL Server 文档**
  
1. [CLR 用户定义类型](/sql/relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types)  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
