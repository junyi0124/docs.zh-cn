---
title: SQL Server 二进制和大值数据
ms.date: 03/30/2017
ms.assetid: e00827b3-7511-4b2d-91d7-851ca86cc6b5
ms.openlocfilehash: a9296e83b0f4e4352423470433670bb2cbe5a248
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183034"
---
# <a name="sql-server-binary-and-large-value-data"></a>SQL Server 二进制和大值数据

SQL Server 提供了 `max` 说明符以扩展 `varchar`、`nvarchar` 和 `varbinary` 数据类型的存储容量。 `varchar(max)`、`nvarchar(max)` 和 `varbinary(max)` 统称为“大值数据类型”**。 你可以使用大值数据类型来存储长达 2^31-1 个字节的数据。  
  
 SQL Server 2008 引入了 FILESTREAM 属性，该属性不是数据类型，而是可以在列上定义的属性，从而允许大值数据存储在文件系统而不是数据库中。  
  
## <a name="in-this-section"></a>本节内容  

 [修改 ADO.NET 中的大值 (最大) 数据](modifying-large-value-max-data.md)  
 说明如何使用大值数据类型。  
  
 [FILESTREAM 数据](filestream-data.md)  
 介绍如何使用 FILESTREAM 属性处理 SQL Server 2008 中存储的大值数据。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 数据类型和 ADO.NET](sql-server-data-types.md)
- [ADO.NET 中的 SQL Server 数据操作](sql-server-data-operations.md)
- [在 ADO.NET 中检索和修改数据](../retrieving-and-modifying-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
