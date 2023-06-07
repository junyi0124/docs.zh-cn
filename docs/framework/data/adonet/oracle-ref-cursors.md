---
title: Oracle REF CURSOR
ms.date: 03/30/2017
ms.assetid: c6b25b8b-0bdd-41b2-9c7c-661f070c2247
ms.openlocfilehash: cbf330ba381a825c2d16038c01d7bdc52bc8f482
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180876"
---
# <a name="oracle-ref-cursors"></a>Oracle REF CURSOR

用于 Oracle 的 .NET Framework 数据提供程序支持 Oracle **REF CURSOR** 数据类型。 在通过数据提供程序使用 Oracle REF CURSOR 时，应考虑下列行为。  
  
> [!NOTE]
> 有些行为与 Microsoft Oracle OLE DB 提供程序 (MSDAORA) 的行为不同。  
  
- 出于性能方面的考虑，Oracle 的数据提供程序不会自动绑定 **REF CURSOR** 数据类型（如 MSDAORA），除非显式指定它们。  
  
- 数据提供程序不支持任何 ODBC 转义序列，包括用于指定 REF CURSOR 参数的 {resultset} 转义。  
  
- 若要执行返回 REF cursor 的存储过程，必须在中定义 <xref:System.Data.OracleClient.OracleParameterCollection> 带有 <xref:System.Data.OracleClient.OracleType> **Cursor** 和 <xref:System.Data.OracleClient.OracleParameter.Direction%2A> **Output**的参数。 数据提供程序只支持作为输出参数绑定 REF CURSOR。 提供程序不支持 REF CURSOR 作为输入参数。  
  
- 不支持从参数值获取 <xref:System.Data.OracleClient.OracleDataReader>。 在执行命令后，值属于 <xref:System.DBNull> 类型。  
  
- 仅当调用) 为 CloseConnection 时，才 (使用 REF 游标的唯一**CommandBehavior**枚举值 <xref:System.Data.OracleClient.OracleCommand.ExecuteReader%2A> ; 将忽略所有其他枚举值。 **CloseConnection**  
  
- **OracleDataReader**中 REF 游标的顺序取决于**OracleParameterCollection**中参数的顺序。 <xref:System.Data.OracleClient.OracleParameter.ParameterName%2A> 属性被忽略。  
  
- PL/SQL **表** 数据类型不受支持。 但是，REF CURSOR 的效率更高。 如果必须使用 **表** 数据类型，请使用 MSDAORA OLE DB 的 .Net 数据提供程序。  
  
## <a name="in-this-section"></a>本节内容  

 [REF CURSOR 示例](ref-cursor-examples.md)  
 包含三个示例，演示如何使用 REF CURSOR。  
  
 [OracleDataReader 中的 REF CURSOR 参数](ref-cursor-parameters-in-an-oracledatareader.md)  
 演示如何执行一个 PL/SQL 存储过程，该存储过程返回 REF CURSOR 参数，并以 **OracleDataReader**的形式读取该值。  
  
 [使用 OracleDataReader 从多个 REF CURSOR 中检索数据](retrieving-data-from-multiple-ref-cursors.md)  
 演示如何执行一个 PL/SQL 存储过程，该存储过程返回两个 REF CURSOR 参数，并使用 **OracleDataReader**读取值。  
  
 [使用一个或多个 REF CURSOR 填充数据集](filling-a-dataset-using-one-or-more-ref-cursors.md)  
 演示如何执行一个 PL/SQL 存储过程，返回两个 REF CURSOR 参数，并使用返回的行填充 <xref:System.Data.DataSet>。  
  
## <a name="see-also"></a>请参阅

- [Oracle 和 ADO.NET](oracle-and-adonet.md)
- [ADO.NET 概述](ado-net-overview.md)
