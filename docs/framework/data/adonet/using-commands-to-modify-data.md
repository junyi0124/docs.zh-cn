---
title: 使用命令修改数据
ms.date: 03/30/2017
ms.assetid: f4160389-b9ff-4b74-b655-437c76dcd586
ms.openlocfilehash: e2f61dbf74f28d026ae91a2bc8f5bb78530ffeac
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177249"
---
# <a name="using-commands-to-modify-data"></a>使用命令修改数据

使用 .NET Framework 数据提供程序，您可以执行存储过程或数据定义语言语句（如 CREATE TABLE 和 ALTER COLUMN）来对数据库或编录执行架构处理。 这些命令不会像查询那样返回行，因此 **Command** 对象提供 **ExecuteNonQuery** 来处理它们。  
  
 除了使用 **ExecuteNonQuery** 修改架构以外，还可以使用此方法来处理修改数据但不返回行的 SQL 语句，如 INSERT、UPDATE 和 DELETE。  
  
 尽管**ExecuteNonQuery**方法不返回行，但输入和输出参数和返回值可通过**命令**对象的**参数**集合进行传递和返回。  
  
## <a name="in-this-section"></a>本节内容  

 [更新数据源中的数据](updating-data-in-a-data-source.md)  
 描述如何执行对数据库中的数据进行修改的命令或存储过程。  
  
 [执行目录操作](performing-catalog-operations.md)  
 描述如何执行对数据库架构进行修改的命令。  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [命令和参数](commands-and-parameters.md)
- [ADO.NET 概述](ado-net-overview.md)
