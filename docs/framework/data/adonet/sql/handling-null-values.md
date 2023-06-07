---
title: 处理 Null 值
description: 了解 SQL Server 的 .NET Framework 数据提供程序如何处理 null，并了解 null 和 SqlBoolean、三值逻辑，并分配 null 值。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f18b288f-b265-4bbe-957f-c6833c0645ef
ms.openlocfilehash: 2dda65f605ea9de616f01d6e52eb4e0e5def4db7
ms.sourcegitcommit: 6d1ae17e60384f3b5953ca7b45ac859ec6d4c3a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94982513"
---
# <a name="handling-null-values"></a>处理 Null 值

如果列中的值未知或缺失，则使用关系数据库中的 null 值。 null 既不是空字符串（对于字符或日期/时间数据类型），也不是零值（对于数值数据类型）。 ANSI SQL-92 规范规定，所有数据类型的 null 值必须相同，以便一致地处理所有 null 值。 <xref:System.Data.SqlTypes> 命名空间通过实现 <xref:System.Data.SqlTypes.INullable> 接口提供 null 语义。 <xref:System.Data.SqlTypes> 中的每个数据类型都有其自己的 `IsNull` 属性和可分配给该数据类型的实例的 `Null` 值。  
  
> [!NOTE]
> .NET Framework 版本2.0 引入了对可以为 null 的值类型的支持，这些类型使程序员能够扩展值类型来表示基础类型的所有值。 这些 CLR 可以为 null 的值类型表示结构的实例 <xref:System.Nullable> 。 当对值类型进行装箱和取消装箱时，此功能特别有用，从而增强了与对象类型的兼容性。 CLR 可以为 null 的值类型不用于存储数据库 null 值，因为 ANSI SQL null 与 `null` 引用 (或 Visual Basic) 的方式不同 `Nothing` 。 若要使用数据库 ANSI SQL null 值，请使用 <xref:System.Data.SqlTypes> null 值，而不是 <xref:System.Nullable>。 若要详细了解如何在 Visual Basic 中使用 CLR 值可以为 null 的类型，请参阅[可以为 null 的值类型](../../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)，对于 c #，请参阅[可以为 null](../../../../csharp/language-reference/builtin-types/nullable-value-types.md)  
  
## <a name="nulls-and-three-valued-logic"></a>空和三值逻辑  

 允许列定义中的 null 值在应用程序中引入了三值逻辑。 比较的计算结果可以为以下三种情况之一：  
  
- True  
  
- False  
  
- 未知  
  
 由于 null 被视为未知，因此两个 null 值之间的比较不被视为相等。 在使用算术运算符的表达式中，如果有任何操作数为 null，则结果也为 null。  
  
## <a name="nulls-and-sqlboolean"></a>null 和 SqlBoolean  

 任何 <xref:System.Data.SqlTypes> 之间的比较将返回一个 <xref:System.Data.SqlTypes.SqlBoolean>。 每个 `IsNull` 的 `SqlType` 函数都返回 <xref:System.Data.SqlTypes.SqlBoolean>，并可用于检查 null 值。 以下真值表显示了 AND、OR 和 NOT 运算符在存在 null 值的情况下的工作方式。 （T=true、F=false 以及 U=未知或 null。）  
  
 ![真值表](./media/truthtable-bpuedev11.gif "TruthTable_bpuedev11")  
  
### <a name="understanding-the-ansi_nulls-option"></a>理解 ANSI_NULLS 选项  

 <xref:System.Data.SqlTypes> 提供与在 SQL Server 中设置 ANSI_NULLS 选项相同的语义。 如果任何操作数或参数为 null （属性除外），则所有算术运算符 (+、-、 \* 、/、% ) 、按位运算符 (~、&、 \|) 和大多数函数都返回 null `IsNull` 。  
  
 ANSI SQL-92 标准不支持 WHERE 子句中的 columnName = NULL  。 在 SQL Server 中，ANSI_NULLS 选项既控制数据库中的默认为 Null 性，又控制对 null 值的比较的求值。 如果打开 ANSI_NULLS（默认值），则在测试 null 值时，必须在表达式中使用 IS NULL 运算符。 例如，当 ANSI_NULLS 为开启状态时，以下比较总是生成未知：  
  
```sql
colname > NULL  
```  
  
 比较包含 null 值的变量也会生成未知：  
  
```sql
colname > @MyVariable  
```  
  
 使用 IS NULL 或 IS NOT NULL 谓词测试 NULL 值。 这将增加 WHERE 子句的复杂性。 例如，AdventureWorks Customer 表中的 TerritoryID 列允许 null 值。 如果 SELECT 语句不仅用于测试其他值，还用于测试 NULL 值，则该语句必须包括 IS NULL 谓词：  
  
```sql
SELECT CustomerID, AccountNumber, TerritoryID  
FROM AdventureWorks.Sales.Customer  
WHERE TerritoryID IN (1, 2, 3)  
   OR TerritoryID IS NULL  
```  
  
 如果在 SQL Server 将 ANSI_NULLS 设置为关闭，则可以创建使用相等运算符与 null 进行比较的表达式。 但是，不能阻止不同的连接为该连接设置 null 选项。 使用 IS NULL 测试 null 值始终有效，而不考虑连接的 ANSI_NULLS 设置。  
  
 不支持在 `DataSet` 中将 ANSI_NULLS 设置为关闭，这始终遵循 ANSI SQL-92 标准来处理 <xref:System.Data.SqlTypes> 中的 null 值。  
  
## <a name="assigning-null-values"></a>赋予 Null 值  

 null 值为特殊值，其存储和赋值语义在不同类型系统和存储系统之间是不同的。 `Dataset` 旨在与不同的类型和存储系统一起使用。  
  
 本部分介绍 null 语义，该语义用于将 null 值分配给不同类型系统中 <xref:System.Data.DataColumn> 的 <xref:System.Data.DataRow>。  
  
 `DBNull.Value`  
 该赋值对于任何类型的 `DataColumn` 都有效。 如果类型实现 `INullable`，则会将 `DBNull.Value` 强制转换为相应的强类型 Null 值。  
  
 `SqlType.Null`  
 所有 <xref:System.Data.SqlTypes> 数据类型都实现 `INullable`。 如果可使用隐式强制转换运算符将强类型的 null 值转换为列的数据类型，则应执行赋值。 否则，将引发无效强制转换异常。  
  
 `null`  
 如果“null”是给定 `DataColumn` 数据类型的合法值，则会将其强制转换为与 `DbNull.Value` 类型 (`Null`) 关联的相应 `INullable` 或 `SqlType.Null`  
  
 `derivedUdt.Null`  
 对于 UDT 列，将始终基于与 `DataColumn` 关联的类型存储 null 值。 请考虑与 `DataColumn` 关联的 UDT 的情况，它不实现 `INullable`，但其子类会实现。 在这种情况下，如果分配了与派生类关联的强类型 null 值，则会将其存储为非类型化的 `DbNull.Value`，因为 null 存储始终与 DataColumn 的数据类型一致。  
  
> [!NOTE]
> `Nullable<T>` 当前不支持 <xref:System.Nullable> 或 `DataSet` 结构。  
  
### <a name="multiple-column-row-assignment"></a>多列（行）赋值  

 `DataTable.Add`、`DataTable.LoadDataRow` 或接受映射到行的 <xref:System.Data.DataRow.ItemArray%2A> 的其他 API，请将“null”映射到 DataColumn 的默认值。 如果数组中的对象包含 `DbNull.Value` 或其强类型对应项，则将应用上述规则。  
  
 此外，以下规则适用于 `DataRow.["columnName"]` null 赋值的实例：  
  
1. *默认* 值为 `DbNull.Value` 除强类型 null 列之外的所有列，其中它是适当的强类型 null 值。  
  
2. 在序列化为 XML 文件时，不会写出 null 值（如在“xsi:nil”中）。  
  
3. 在序列化为 XML 时，始终写出所有非 null 值（包括默认值）。 这不同于 XSD/XML 语义，其中 null 值 (xsi:nil) 是显式的，而默认值是隐式的（如果在 XML 中不存在，验证分析器可以从关联的 XSD 架构中获取它）。 `DataTable` 则是相反：null 值是隐式的，而默认值是显式的。  
  
4. 从 XML 输入读取的行的所有缺少列值都将分配为 NULL。 将向使用 <xref:System.Data.DataTable.NewRow%2A> 或类似方法创建的行分配 DataColumn 的默认值。  
  
5. <xref:System.Data.DataRow.IsNull%2A> 方法为 `true` 和 `DbNull.Value` 返回 `INullable.Null`。  
  
## <a name="assigning-null-values"></a>赋予 Null 值  

 任何 <xref:System.Data.SqlTypes> 实例的默认值为 null。  
  
 <xref:System.Data.SqlTypes> 中的 null 值为特定类型，不能由单个值表示，如 `DbNull`。 使用 `IsNull` 属性检查 null 值。  
  
 null 值可以分配给 <xref:System.Data.DataColumn>，如下面的代码示例中所示。 可以直接将 null 值分配给 `SqlTypes` 变量，而不会触发异常。  
  
### <a name="example"></a>示例  

 下面的代码示例创建一个 <xref:System.Data.DataTable>，其中两列定义为 <xref:System.Data.SqlTypes.SqlInt32> 和 <xref:System.Data.SqlTypes.SqlString>。 该代码将添加一行已知值、一行 null 值，然后循环访问 <xref:System.Data.DataTable>，进而将值分配给变量并在控制台窗口中显示输出。  
  
 [!code-csharp[DataWorks SqlTypes.IsNull#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTypes.IsNull/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTypes.IsNull#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTypes.IsNull/VB/source.vb#1)]  
  
 此示例显示以下结果：  
  
```output
isColumnNull=False, ID=123, Description=Side Mirror  
isColumnNull=True, ID=Null, Description=Null  
```  
  
## <a name="comparing-null-values-with-sqltypes-and-clr-types"></a>将空值与 SqlTypes 和 CLR 类型进行比较  

 比较 null 值时，务必要了解 `Equals` 方法计算 <xref:System.Data.SqlTypes> 中 null 值的方式与它在 CLR 类型中的工作方式相比之间的差异。 所有 <xref:System.Data.SqlTypes>`Equals` 方法都使用数据库语义计算 null 值：如果两个值中的一个或两个都为 null，则比较结果为 null。 另一方面，如果两个 `Equals` 都为 null，则对两者使用 CLR <xref:System.Data.SqlTypes> 方法将生成 true。 这反映了使用实例方法（如 CLR `String.Equals` 方法）和使用静态/共享方法 (`SqlString.Equals`) 的差异。  
  
 下面的示例演示了 `SqlString.Equals` 方法与 `String.Equals` 方法在分别传递一对 null 值和一对空字符串时的结果差异。  
  
 [!code-csharp[DataWorks SqlTypes.CompareNulls#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTypes.CompareNulls/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTypes.CompareNulls#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTypes.CompareNulls/VB/source.vb#1)]  
  
 此代码将产生以下输出：  
  
```output
SqlString.Equals shared/static method:  
  Two nulls=Null  
  
String.Equals instance method:  
  Two nulls=True  
  
SqlString.Equals shared/static method:  
  Two empty strings=True  
  
String.Equals instance method:  
  Two empty strings=True
```  
  
## <a name="see-also"></a>另请参阅

- [SQL Server 数据类型和 ADO.NET](sql-server-data-types.md)
- [ADO.NET 概述](../ado-net-overview.md)
