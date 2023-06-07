---
title: 配置参数和参数数据类型
description: 命令对象使用参数将值传递给 SQL 语句或存储过程，在 ADO.NET 中提供类型检查和验证。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.openlocfilehash: a426eeae785274b0484a84a2fae2dce4572fb4c4
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287111"
---
# <a name="configuring-parameters-and-parameter-data-types"></a>配置参数和参数数据类型

通过提供类型检查和验证，命令对象可使用参数来将值传递给 SQL 语句或存储过程。 与命令文本不同，参数输入被视为文本值，而不是可执行代码。 这样可帮助抵御“SQL 注入”攻击，这种攻击的攻击者会将命令插入 SQL 语句，从而危及服务器的安全。

参数化命令还可提高查询执行性能，因为它们可帮助数据库服务器将传入命令与适当的缓存查询计划进行准确匹配。 有关详细信息，请参阅[执行计划缓存和重用](/sql/relational-databases/query-processing-architecture-guide#execution-plan-caching-and-reuse)以及[参数和执行计划重用](/sql/relational-databases/query-processing-architecture-guide#PlanReuse)。 除具备安全和性能优势外，参数化命令还提供一种用于组织传递到数据源的值的便捷方法。

<xref:System.Data.Common.DbParameter> 对象可以通过使用其构造函数来创建，或者也可以通过调用 <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> 集合的 `Add` 方法以将该对象添加到 <xref:System.Data.Common.DbParameterCollection> 来创建。 `Add` 方法将构造函数实参或现有形参对象用作输入，具体取决于数据提供程序。

## <a name="supplying-the-parameterdirection-property"></a>提供 ParameterDirection 属性

在添加参数时，您必须为输入参数以外的参数提供一个 <xref:System.Data.ParameterDirection> 属性。 下表显示了可用于 `ParameterDirection` 枚举的 <xref:System.Data.ParameterDirection> 值。

|成员名称|说明|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|该参数为输入参数。 这是默认设置。|
|<xref:System.Data.ParameterDirection.InputOutput>|该参数可执行输入和输出。|
|<xref:System.Data.ParameterDirection.Output>|该参数为输出参数。|
|<xref:System.Data.ParameterDirection.ReturnValue>|该参数表示从某操作（如存储过程、内置函数或用户定义的函数）返回的值。|

## <a name="working-with-parameter-placeholders"></a>使用参数占位符

参数占位符的语法取决于数据源。 .NET Framework 数据提供程序以不同方式处理命名和指定参数和参数占位符。 此语法是针对某个特定的数据源自定义的，如下表所述。

|数据提供程序|参数命名语法|
|-------------------|-----------------------------|
|<xref:System.Data.SqlClient>|以 `@`*参数名*格式使用命名参数。|
|<xref:System.Data.OleDb>|使用由问号 (`?`) 指示的位置参数标记。|
|<xref:System.Data.Odbc>|使用由问号 (`?`) 指示的位置参数标记。|
|<xref:System.Data.OracleClient>|以 `:`*参数名* （或 *参数名*）格式使用命名参数。|

## <a name="specifying-parameter-data-types"></a>指定参数数据类型

参数的数据类型特定于 .NET Framework 的数据访问接口。 在将值传递给数据源之前，指定类型将的值转换 `Parameter` 为 .NET Framework 数据提供程序类型。 也可以通过通用的方式指定 `Parameter` 的类型，方法是将 `DbType` 对象的 `Parameter` 属性设置为特定的 <xref:System.Data.DbType>。

对象的 .NET Framework 数据提供程序类型 `Parameter` 是从对象的的 .NET Framework 类型 `Value` `Parameter` 或从对象的来推断的 `DbType` `Parameter` 。 下表显示了根据作为 `Parameter` 值传递的对象或指定的 `Parameter` 推断出的 `DbType`类型。

|.NET Framework 类型|DbType|SqlDbType|OleDbType|OdbcType|OracleType|
|-------------------------|------------|---------------|---------------|--------------|----------------|
|<xref:System.Boolean>|Boolean|bit|Boolean|bit|Byte|
|<xref:System.Byte>|Byte|TinyInt|UnsignedTinyInt|TinyInt|Byte|
|byte[]|二进制|VarBinary. 如果字节数组大于 VarBinary 的最大大小（8000字节），此隐式转换将失败。对于大于8000字节的字节数组，请显式设置 <xref:System.Data.SqlDbType> 。|VarBinary|二进制|原始|
|<xref:System.Char>| |不支持从 char 推断 <xref:System.Data.SqlDbType> 。|Char|Char|Byte|
|<xref:System.DateTime>|DateTime|DateTime|DBTimeStamp|DateTime|DateTime|
|<xref:System.DateTimeOffset>|DateTimeOffset|SQL Server 2008 中的 DateTimeOffset。 SQL Server 2008 以前的 SQL Server 版本不支持从 DateTimeOffset 推断 <xref:System.Data.SqlDbType> 。|||DateTime|
|<xref:System.Decimal>|小数|小数|小数|Numeric|Number|
|<xref:System.Double>|Double|Float|Double|Double|Double|
|<xref:System.Single>|Single|Real|Single|Real|Float|
|<xref:System.Guid>|Guid|UniqueIdentifier|Guid|UniqueIdentifier|原始|
|<xref:System.Int16>|Int16|SmallInt|SmallInt|SmallInt|Int16|
|<xref:System.Int32>|Int32|int|int|int|Int32|
|<xref:System.Int64>|Int64|BigInt|BigInt|BigInt|Number|
|<xref:System.Object>|对象|变量|变量|不支持从 Object 推断 OdbcType。|Blob|
|<xref:System.String>|String|NVarChar。 如果字符串大于 NVarChar 的最大大小（4000 个字符），此隐式转换将失败。 对于大于 4000 个字符的字符串，请显式设置 <xref:System.Data.SqlDbType>。|VarWChar|NVarChar|NVarChar|
|<xref:System.TimeSpan>|时间|SQL Server 2008 中的 Time。 SQL Server 2008 以前的 SQL Server 版本不支持从 TimeSpan 推断 <xref:System.Data.SqlDbType> 。|DBTime|时间|DateTime|
|<xref:System.UInt16>|UInt16|不支持从 UInt16 推断 <xref:System.Data.SqlDbType> 。|UnsignedSmallInt|int|UInt16|
|<xref:System.UInt32>|UInt32|不支持从 UInt32 推断 <xref:System.Data.SqlDbType> 。|UnsignedInt|BigInt|UInt32|
|<xref:System.UInt64>|UInt64|不支持从 UInt64 推断 <xref:System.Data.SqlDbType> 。|UnsignedBigInt|数字|Number|
||AnsiString|VarChar|VarChar|VarChar|VarChar|
||AnsiStringFixedLength|Char|Char|Char|Char|
||货币|Money|货币|不支持从 `OdbcType` 推断 `Currency` 。|Number|
||日期|SQL Server 2008 中的 Date。 SQL Server 2008 以前的 SQL Server 版本不支持从 Date 推断 <xref:System.Data.SqlDbType> 。|DBDate|Date|DateTime|
||SByte|不支持从 SByte 推断 <xref:System.Data.SqlDbType> 。|TinyInt|不支持从 SByte 推断 `OdbcType` 。|SByte|
||StringFixedLength|NChar|WChar|NChar|NChar|
||时间|SQL Server 2008 中的 Time。 SQL Server 2008 以前的 SQL Server 版本不支持从 Time 推断 <xref:System.Data.SqlDbType> 。|DBTime|时间|DateTime|
||VarNumeric|不支持从 VarNumeric 推断 <xref:System.Data.SqlDbType> 。|VarNumeric|不支持从 VarNumeric 推断 `OdbcType` 。|Number|
|用户定义类型（带有 <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>的对象）|对象或字符串，具体取决于提供程序（SqlClient 始终返回对象，Odbc 始终返回字符串，而 OleDb 托管数据提供程序可查看两者中的任何一个|如果存在 <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute> ，则为 SqlDbType.Udt；否则为 Variant|OleDbType.VarWChar（如果值为 null），否则为 OleDbType.Variant。|OdbcType.NVarChar|不受支持|

> [!NOTE]
> 从小数转换到其他类型是缩窄转换，这种转换会将小数值舍入到最近的接近零的整数值。 如果无法以目标类型表示转换结果，则会引发 <xref:System.OverflowException> 。

> [!NOTE]
> 将 null 参数值发送到服务器时，必须指定 <xref:System.DBNull> ，而不是 `null` （ `Nothing` 在 Visual Basic 中）。 系统中的 NULL 值是没有值的空对象。 <xref:System.DBNull> 用于表示 NULL 值。 有关数据库 null 值的详细信息，请参阅[处理 Null 值](./sql/handling-null-values.md)。

## <a name="deriving-parameter-information"></a>派生参数信息

还可以使用 `DbCommandBuilder` 类从存储过程派生参数。 `SqlCommandBuilder` 和 `OleDbCommandBuilder` 类都提供了静态方法 `DeriveParameters`，该静态方法将自动使用存储过程中的参数信息填充 Command 对象的 Parameters 集合。 请注意， `DeriveParameters` 会覆盖此命令的任何现有参数信息。

> [!NOTE]
> 派生参数信息会影响性能，因为它需要对数据源进行额外的往返访问，以检索信息。 如果参数信息在设计时是已知的，则可以通过显式设置参数来提高应用程序的性能。

有关详细信息，请参阅[通过 Commandbuilder 生成命令](generating-commands-with-commandbuilders.md)。

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>对 SqlCommand 和存储过程使用参数

在数据驱动的应用程序中，存储过程具有许多优势。 通过利用存储过程，数据库操作可以包装在单个命令中，为获取最佳性能而进行优化并通过附加的安全性得到增强。 尽管可以通过将后跟参数参数的存储过程名称作为 SQL 语句传递来调用存储过程，但通过使用 <xref:System.Data.Common.DbCommand.Parameters%2A> ADO.NET 对象的集合， <xref:System.Data.Common.DbCommand> 您可以更明确地定义存储过程参数，并访问输出参数和返回值。

> [!NOTE]
> 参数化语句在服务器上通过使用 `sp_executesql,` 执行，sp_executesql 允许重复使用查询计划。 `sp_executesql` 批处理命令中的本地光标或变量对于调用 `sp_executesql`的批处理命令是不可见的。 数据库上下文中的更改只持续到 `sp_executesql` 语句的结尾。 有关详细信息，请参阅[sp_executesql （transact-sql）](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql)。

对 <xref:System.Data.SqlClient.SqlCommand> 使用参数以执行 SQL Server 存储过程时，添加到 <xref:System.Data.SqlClient.SqlCommand.Parameters%2A> 集合中的参数的名称必须与存储过程中参数标记的名称相匹配。 SQL Server 的 .NET Framework 数据提供程序不支持用于向 SQL 语句或存储过程传递参数的问号（？）占位符。 它将存储过程中的参数视为命名参数，并搜索匹配的参数标记。 例如，通过使用名为 `CustOrderHist` 的参数定义 `@CustomerID`存储过程。 您的代码在执行该存储过程时，它也必须使用名为 `@CustomerID`的参数。

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>示例

此示例演示了如何调用 `Northwind` 示例数据库中的 SQL Server 存储过程。 存储过程的名称为 `dbo.SalesByCategory` ，它具有名为 `@CategoryName` 的输入参数，其数据类型为 `nvarchar(15)`。 该代码在 using 代码块内创建一个新 <xref:System.Data.SqlClient.SqlConnection> ，以便在过程结束时释放连接。 会创建 <xref:System.Data.SqlClient.SqlCommand> 和 <xref:System.Data.SqlClient.SqlParameter> 对象，并设置其属性。 <xref:System.Data.SqlClient.SqlDataReader> 会执行 `SqlCommand` 并从存储过程返回结果集，以在控制台窗口中显示相关输出。

> [!NOTE]
> 您可以选择使用任一重载构造函数在一个语句中设置多个属性，而不是创建 `SqlCommand` 和 `SqlParameter` 对象，然后在各个语句中设置属性。

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.StoredProcedure/CS/source.cs#1)]
[!code-vb[DataWorks SqlClient.StoredProcedure#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.StoredProcedure/VB/source.vb#1)]

## <a name="using-parameters-with-an-oledbcommand-or-odbccommand"></a>将参数与 OleDbCommand 或 OdbcCommand 一起使用

对 <xref:System.Data.OleDb.OleDbCommand> 或 <xref:System.Data.Odbc.OdbcCommand>使用参数时，添加到 `Parameters` 集合中的参数的顺序必须与在存储过程中定义的参数的顺序相匹配。 用于 ODBC 的 OLE DB 和 .NET Framework 数据提供程序的 .NET Framework 数据提供程序将存储过程中的参数视为占位符并按顺序应用参数值。 此外，返回值参数必须为添加到 `Parameters` 集合中的第一批参数。

用于 ODBC 的 OLE DB 和 .NET Framework 数据提供程序的 .NET Framework 数据提供程序不支持将参数传递给 SQL 语句或存储过程的命名参数。 在此情况下，必须使用问号 (?) 占位符，如以下示例所示。

```sql
SELECT * FROM Customers WHERE CustomerID = ?
```

因此，将 `Parameter` 对象添加到 `Parameters` 集合的顺序必须直接与参数的问号 (?) 占位符的位置相对应。

### <a name="oledb-example"></a>OleDb 示例

```vb
Dim command As OleDbCommand = New OleDbCommand( _
  "SampleProc", connection)
command.CommandType = CommandType.StoredProcedure

Dim parameter As OleDbParameter = command.Parameters.Add( _
  "RETURN_VALUE", OleDbType.Integer)
parameter.Direction = ParameterDirection.ReturnValue

parameter = command.Parameters.Add( _
  "@InputParm", OleDbType.VarChar, 12)
parameter.Value = "Sample Value"

parameter = command.Parameters.Add( _
  "@OutputParm", OleDbType.VarChar, 28)
parameter.Direction = ParameterDirection.Output
```

```csharp
OleDbCommand command = new OleDbCommand("SampleProc", connection);
command.CommandType = CommandType.StoredProcedure;

OleDbParameter parameter = command.Parameters.Add(
  "RETURN_VALUE", OleDbType.Integer);
parameter.Direction = ParameterDirection.ReturnValue;

parameter = command.Parameters.Add(
  "@InputParm", OleDbType.VarChar, 12);
parameter.Value = "Sample Value";

parameter = command.Parameters.Add(
  "@OutputParm", OleDbType.VarChar, 28);
parameter.Direction = ParameterDirection.Output;
```

## <a name="odbc-example"></a>Odbc 示例

```vb
Dim command As OdbcCommand = New OdbcCommand( _
  "{ ? = CALL SampleProc(?, ?) }", connection)
command.CommandType = CommandType.StoredProcedure

Dim parameter As OdbcParameter = command.Parameters.Add("RETURN_VALUE", OdbcType.Int)
parameter.Direction = ParameterDirection.ReturnValue

parameter = command.Parameters.Add( _
  "@InputParm", OdbcType.VarChar, 12)
parameter.Value = "Sample Value"

parameter = command.Parameters.Add( _
  "@OutputParm", OdbcType.VarChar, 28)
parameter.Direction = ParameterDirection.Output
```

```csharp
OdbcCommand command = new OdbcCommand( _
  "{ ? = CALL SampleProc(?, ?) }", connection);
command.CommandType = CommandType.StoredProcedure;

OdbcParameter parameter = command.Parameters.Add( _
  "RETURN_VALUE", OdbcType.Int);
parameter.Direction = ParameterDirection.ReturnValue;

parameter = command.Parameters.Add( _
  "@InputParm", OdbcType.VarChar, 12);
parameter.Value = "Sample Value";

parameter = command.Parameters.Add( _
  "@OutputParm", OdbcType.VarChar, 28);
parameter.Direction = ParameterDirection.Output;
```

## <a name="see-also"></a>另请参阅

- [命令和参数](commands-and-parameters.md)
- [DataAdapter 参数](dataadapter-parameters.md)
- [ADO.NET 中的数据类型映射](data-type-mappings-in-ado-net.md)
- [ADO.NET 概述](ado-net-overview.md)
