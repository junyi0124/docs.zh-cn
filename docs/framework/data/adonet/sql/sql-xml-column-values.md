---
title: SQL XML 列值
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d97ce4da-f09c-4d1e-85b7-a0ccedd7246a
ms.openlocfilehash: cd55e2263d4b71fe62910ac918e331ebe37833eb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177275"
---
# <a name="sql-xml-column-values"></a>SQL XML 列值

SQL Server 支持 `xml` 数据类型，开发人员可以使用 <xref:System.Data.SqlClient.SqlCommand> 类的标准行为检索包括此类型的结果集。 可以检索 `xml` 列（例如，检索到 <xref:System.Data.SqlClient.SqlDataReader> 中），就像检索任何列一样；但若要以 XML 格式处理列内容，必须使用 <xref:System.Xml.XmlReader>。  
  
## <a name="example"></a>示例  

 以下控制台应用程序从 AdventureWorks 数据库的 Sales.Store 表中为 <xref:System.Data.SqlClient.SqlDataReader> 实例选择两行，每行包含一个 `xml` 列********。 对于每一行，`xml` 列的值是使用 <xref:System.Data.SqlClient.SqlDataReader> 的 <xref:System.Data.SqlClient.SqlDataReader.GetSqlXml%2A> 方法进行读取。 此值存储在 <xref:System.Xml.XmlReader> 中。 请注意，若要将内容设置为 <xref:System.Data.SqlTypes.SqlXml> 变量，必须使用 <xref:System.Data.SqlClient.SqlDataReader.GetSqlXml%2A>，而不是 <xref:System.Data.IDataRecord.GetValue%2A> 方法；<xref:System.Data.IDataRecord.GetValue%2A> 以字符串形式返回 `xml` 列的值。  
  
> [!NOTE]
> 默认情况下，在安装 SQL Server 时不安装 AdventureWorks 示例数据库****。 可以通过运行 SQL Server 安装程序来安装它。  
  
 [!code-csharp[DataWorks SqlClient.GetXmlDataReader#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.GetXmlDataReader/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.GetXmlDataReader#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.GetXmlDataReader/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.SqlTypes.SqlXml>
- [SQL Server 中的 XML 数据](xml-data-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
