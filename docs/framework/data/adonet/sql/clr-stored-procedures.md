---
title: CLR 存储过程
ms.date: 03/30/2017
ms.assetid: fd7eea9b-218a-4988-8c9a-8abcc6031c66
ms.openlocfilehash: aa14c96ed226ac80a9613d3e229f35dbf5085f3b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173609"
---
# <a name="clr-stored-procedures"></a>CLR 存储过程

存储过程是不能用于标量表达式的例程。 它们可以将表格形式的结果和消息返回到客户端，调用数据定义语言 (DDL) 和数据操作语言 (DML) 语句，以及返回输出参数。  
  
> [!NOTE]
> Microsoft Visual Basic 不支持采用与 Microsoft Visual C# 中相同的方式处理输出参数。 您必须指定以通过引用传递参数并应用 \<Out()> 属性以表示输出参数，如下所示：  
  
```vb
Public Shared Sub ExecuteToClient( <Out()> ByRef number As Integer)  
```
  
有关更多详细信息，请参阅所使用 SQL Server 版本的 [SQL Server 文档](/sql) 版本。
  
 **SQL Server 文档**

1. [CLR 存储过程](/previous-versions/sql/sql-server-2008/ms131094(v=sql.100))  
  
## <a name="see-also"></a>请参阅

- [在托管代码中创建 SQL Server 2005 对象](/previous-versions/visualstudio/visual-studio-2008/6s0s2at1(v=vs.90))
- [ADO.NET 概述](../ado-net-overview.md)
