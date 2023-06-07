---
title: 大型 UDT
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 420ae24e-762b-4e09-b4c3-2112c470ee49
ms.openlocfilehash: 032093244f51893cd3b0cf50ad81c79413aaa32e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194539"
---
# <a name="large-udts"></a>大型 UDT

用户定义类型 (UDT) 使开发人员可以通过在 SQL Server 数据库中存储公共语言运行时 (CLR) 对象来扩展服务器的标量类型系统。 UDT 可以包含多个元素，也可以具有多种行为，不同于传统的由单个 SQL Server 系统数据类型组成的别名数据类型。  
  
> [!NOTE]
> 必须安装 .NET Framework 3.5 SP1（或更高版本）才能利用针对大型 UDT 增强的 SqlClient 支持。  
  
 以前 UDT 的最大大小限制为 8 KB。 在 SQL Server 2008 中，对于 <xref:Microsoft.SqlServer.Server.Format.UserDefined> 格式的 UDT，已不再进行此限制。  
  
 有关用户定义类型的完整文档，请参见与您所使用的 SQL Server 版本对应的 SQL Server 联机丛书。  
  
 **SQL Server 文档**  
  
1. [CLR 用户定义类型](/sql/relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types)  
  
## <a name="retrieving-udt-schemas-using-getschema"></a>使用 GetSchema 检索 UDT 架构  

 <xref:System.Data.SqlClient.SqlConnection> 的 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> 方法在 <xref:System.Data.DataTable> 中返回数据库架构信息。 有关详细信息，请参阅 [SQL Server 架构集合](../sql-server-schema-collections.md)。  
  
### <a name="getschematable-column-values-for-udts"></a>UDT 的 GetSchemaTable 列值  

 <xref:System.Data.SqlClient.SqlDataReader> 的 <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A> 方法返回描述列元数据的 <xref:System.Data.DataTable>。 下表描述了 SQL Server 2005 和 SQL Server 2008 之间大型 UDT 的列元数据的区别。  
  
|SqlDataReader 列|SQL Server 2005|SQL Server 2008 和更高版本|  
|--------------------------|---------------------|-------------------------------|  
|`ColumnSize`|多种多样|多种多样|  
|`NumericPrecision`|255|255|  
|`NumericScale`|255|255|  
|`DataType`|`Byte[]`|UDT 实例|  
|`ProviderSpecificDataType`|`SqlTypes.SqlBinary`|UDT 实例|  
|`ProviderType`|21 (`SqlDbType.VarBinary`)|29 (`SqlDbType.Udt`)|  
|`NonVersionedProviderType`|29 (`SqlDbType.Udt`)|29 (`SqlDbType.Udt`)|  
|`DataTypeName`|`SqlDbType.VarBinary`|以 Database.SchemaName.TypeName 形式指定的由三部分组成的名称**。|  
|`IsLong`|多种多样|多种多样|  
  
## <a name="sqldatareader-considerations"></a>SqlDataReader 注意事项  

 <xref:System.Data.SqlClient.SqlDataReader> 在 SQL Server 2008 中已得到扩展，可支持检索大型 UDT 值。 <xref:System.Data.SqlClient.SqlDataReader> 处理多少 UDT 值取决于所使用的 SQL Server 版本以及连接字符串中指定的 `Type System Version`。 有关详细信息，请参阅 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>。  
  
 将 `Type System Version` 设置为 SQL Server 2005 时，以下 <xref:System.Data.SqlClient.SqlDataReader> 方法将返回 <xref:System.Data.SqlTypes.SqlBinary>，而不是 UDT：  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificFieldType%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValues%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetSqlValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetSqlValues%2A>  
  
 将 `Type System Version` 设置为 SQL Server 2005 时，以下方法将返回 `Byte[]` 的数组，而不是 UDT：  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetValues%2A>  
  
 注意，未对当前版本的 ADO.NET 执行转换。  
  
## <a name="specifying-sqlparameters"></a>指定 SqlParameters  

 已扩展以下 <xref:System.Data.SqlClient.SqlParameter> 属性以适用于大型 UDT。  
  
|SqlParameter 属性|描述|  
|---------------------------|-----------------|  
|<xref:System.Data.SqlClient.SqlParameter.Value%2A>|获取或设置表示参数值的对象。 默认值为 NULL。 此属性可以是 `SqlBinary`、`Byte[]` 或托管对象。|  
|<xref:System.Data.SqlClient.SqlParameter.SqlValue%2A>|获取或设置表示参数值的对象。 默认值为 NULL。 此属性可以是 `SqlBinary`、`Byte[]` 或托管对象。|  
|<xref:System.Data.SqlClient.SqlParameter.Size%2A>|获取或设置要解析的参数值的大小。 默认值为 0。 此属性可以是一个表示参数值大小的整数。 对于大型 UDT，此属性可以是 UDT 的实际大小，也可以是 -1，表示未知。|  
  
## <a name="retrieving-data-example"></a>检索数据示例  

 下面的代码段演示如何检索大型 UDT 数据。 `connectionString` 变量假定到 SQL Server 数据库的连接有效，而 `commandString` 变量假定 SELECT 语句有效，其中主键列在最前面。  
  
```csharp  
using (SqlConnection connection = new SqlConnection(
    connectionString, commandString))  
{  
  connection.Open();  
  SqlCommand command = new SqlCommand(commandString);  
  SqlDataReader reader = command.ExecuteReader();  
  while (reader.Read())  
  {  
    // Retrieve the value of the Primary Key column.  
    int id = reader.GetInt32(0);  
  
    // Retrieve the value of the UDT.  
    LargeUDT udt = (LargeUDT)reader[1];  
  
    // You can also use GetSqlValue and GetValue.  
    // LargeUDT udt = (LargeUDT)reader.GetSqlValue(1);  
    // LargeUDT udt = (LargeUDT)reader.GetValue(1);  
  
    Console.WriteLine(  
     "ID={0} LargeUDT={1}", id, udt);  
  }  
reader.close  
}  
```  
  
```vb  
Using connection As New SqlConnection( _  
    connectionString, commandString)  
    connection.Open()  
    Dim command As New SqlCommand(commandString, connection)  
    Dim reader As SqlDataReader  
    reader = command.ExecuteReader  
  
    While reader.Read()  
      ' Retrieve the value of the Primary Key column.  
      Dim id As Int32 = reader.GetInt32(0)  
  
      ' Retrieve the value of the UDT.  
      Dim udt As LargeUDT = CType(reader(1), LargeUDT)  
  
     ' You can also use GetSqlValue and GetValue.  
     ' Dim udt As LargeUDT = CType(reader.GetSqlValue(1), LargeUDT)  
     ' Dim udt As LargeUDT = CType(reader.GetValue(1), LargeUDT)  
  
      ' Print values.  
      Console.WriteLine("ID={0} LargeUDT={1}", id, udt)  
    End While  
    reader.Close()  
End Using  
```  
  
## <a name="see-also"></a>请参阅

- [配置参数和参数数据类型](../configuring-parameters-and-parameter-data-types.md)
- [检索数据库架构信息](../retrieving-database-schema-information.md)
- [SQL Server 数据类型映射](../sql-server-data-type-mappings.md)
- [SQL Server 二进制和大值数据](sql-server-binary-and-large-value-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
