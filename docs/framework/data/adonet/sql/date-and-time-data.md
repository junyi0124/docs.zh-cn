---
title: 日期和时间数据
description: 了解用于处理 SQL Server 的 .NET Framework 数据提供程序中的日期和时间信息的数据类型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.openlocfilehash: 6fe047fc672a2b42f886e81dcace91042a552932
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156311"
---
# <a name="date-and-time-data"></a>日期和时间数据

SQL Server 2008 引入了新数据类型，用于处理日期和时间信息。 新数据类型包括独立的日期和时间类型，以及具有更大范围、精度且时区感知的扩展数据类型。 从 .NET Framework 3.5 Service Pack (SP) 1 开始，适用于 SQL Server 的 .NET Framework 数据提供程序 (<xref:System.Data.SqlClient>) 完全支持 SQL Server 2008 数据库引擎的所有新功能。 您必须安装 .NET Framework 3.5 SP1（或更高版本）才能将这些新功能与 SqlClient 一起使用。  
  
 SQL Server 2008 之前的 SQL Server 版本仅具有两种用于处理日期和时间值的数据类型：`datetime` 和 `smalldatetime`。 这两种数据类型都同时包含日期值和时间值，难以仅使用日期值或仅使用时间值。 此外，这些数据类型只支持在 1753 年英国引入公历之后的日期。 另一个限制是，这些旧数据类型无法感知时区，这使得处理来自多个时区的数据变得困难。  
  
 SQL Server 联机丛书中有关于 SQL Server 数据类型的完整文档。 下表列出了有关日期和时间数据的因版本而异的入门级主题。  
  
 **SQL Server 文档**  
  
1. [使用日期和时间数据](/previous-versions/sql/sql-server-2008/ms180878(v=sql.100))  
  
## <a name="datetime-data-types-introduced-in-sql-server-2008"></a>SQL Server 2008 中引入的日期/时间数据类型  

 下表介绍了新日期和时间数据类型。  
  
|SQL Server 数据类型|描述|  
|--------------------------|-----------------|  
|`date`|`date` 数据类型的范围从 01 年 1 月 1 日 9999 年 12 月 31 日，精确到 1 天。 默认值为 January 1, 1900。 存储大小为 3 个字节。|  
|`time`|`time` 数据类型仅采用 24 小时制存储时间值。 `time` 数据类型介于 00:00:00.0000000 到 23:59:59.9999999 范围之间，准确度为 100 纳秒。 默认值为 00:00:00.0000000（午夜）。 `time` 数据类型支持用户定义的小数秒精度，存储大小根据指定的精度在 3 字节到 6 字节之间变化。|  
|`datetime2`|`datetime2` 数据类型将 `date` 和 `time` 数据类型的范围和精度合并为一个数据类型。<br /><br /> 默认值和字符串文本格式与 `date` 和 `time` 数据类型中定义的相同。|  
|`datetimeoffset`|`datetimeoffset` 数据类型具有 `datetime2` 的所有功能，并添加了时区偏移量。 时区偏移量表示为 [+&#124;-] HH:MM。 HH 是两位数，介于 00 和 14 范围之间，表示时区偏移量中的小时数。 MM 是两位数，范围为 00 到 59，表示时区偏移量中的额外分钟数。 时间格式支持到 100 毫微秒。 必需符号 + 或 - 指明是在 UTC（协调世界时或格林威治标准时间）中加上还是减去时区偏移来获得本地时间。|  
  
> [!NOTE]
> 要详细了解如何使用 `Type System Version`，请参阅 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>。  
  
## <a name="date-format-and-date-order"></a>日期格式和日期顺序  

 SQL Server 如何分析日期和时间值不仅取决于类型系统版本和服务器版本，还取决于服务器的默认语言和格式设置。 如果日期格式使用一种语言，而查询是通过使用不同语言和日期格式设置的连接执行的，那么可能无法识别采用日期格式的日期字符串。  
  
 Transact-SQL SET LANGUAGE 语句隐式设置确定日期部分顺序的 DATEFORMAT。 可以通过按 MDY、DMY、YMD、YDM、MYD 或 DYM 顺序对日期部分进行排序，对连接使用 SET DATEFORMAT Transact-SQL 语句来区分日期值。  
  
 如果你没有为连接指定任何 DATEFORMAT，SQL Server 会使用与连接关联的默认语言。 例如，为日期字符串“01/02/03”在语言设置为美式英语的服务器上解析为 MDY（2003 年 1 月 2 日），而在语言设置为英式英语的服务器上则解析为 DMY（2003 年 2 月 1 日）。 年份是由 SQL Server 的临界年份规则确定的，此规则定义了用于指定世纪值的临界日期。 有关详细信息，请参阅 [两位数年份截止选项](/sql/database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option)。  
  
> [!NOTE]
> 从字符串格式转换为 `date`、`time`、`datetime2` 或 `datetimeoffset` 时，不支持使用 YDM 日期格式。  
  
 有关 SQL Server 如何解释日期和时间数据的详细信息，请参阅 [使用日期和时间数据](/previous-versions/sql/sql-server-2008/ms180878(v=sql.100))。  
  
## <a name="datetime-data-types-and-parameters"></a>日期/时间数据类型和参数  

 为了支持新日期和时间数据类型，已向 <xref:System.Data.SqlDbType> 中添加了下列枚举。  
  
- `SqlDbType.Date`  
  
- `SqlDbType.Time`  
  
- `SqlDbType.DateTime2`  
  
- `SqlDbType.DateTimeOffSet`  

通过使用 <xref:System.Data.SqlDbType> 枚举之一，可以指定 <xref:System.Data.SqlClient.SqlParameter> 的数据类型。

> [!NOTE]
> 无法将 `SqlParameter` 的 `DbType` 属性设置为 `SqlDbType.Date`。

 也可以采用通用的方式通过将 `SqlParameter` 对象的 <xref:System.Data.SqlClient.SqlParameter.DbType%2A> 属性设置为特定的 <xref:System.Data.DbType> 枚举值来指定 <xref:System.Data.SqlClient.SqlParameter> 的类型。 <xref:System.Data.DbType> 中已添加了下面的枚举值，以支持 `datetime2` 和 `datetimeoffset` 数据类型：  
  
- DbType.DateTime2  
  
- DbType.DateTimeOffset  
  
 这些新枚举补充了早期版本的 .NET Framework 中存在的 `Date`、`Time` 和 `DateTime` 枚举。  
  
 某一参数对象的 .NET Framework 数据提供程序类型是从该参数对象的 .NET Framework 类型或该参数对象的 `DbType` 推断而来的。 没有引入新 <xref:System.Data.SqlTypes> 数据类型来支持新日期和时间数据类型。 下表介绍了 SQL Server 2008 日期和时间数据类型与 CLR 数据类型之间的映射。  
  
|SQL Server 数据类型|.NET Framework 类型|System.Data.SqlDbType|System.Data.DbType|  
|--------------------------|-------------------------|---------------------------|------------------------|  
|date|System.DateTime|Date|Date|  
|time|System.TimeSpan|时间|时间|  
|datetime2|System.DateTime|DateTime2|DateTime2|  
|datetimeoffset|System.DateTimeOffset|DateTimeOffset|DateTimeOffset|  
|datetime|System.DateTime|DateTime|DateTime|  
|smalldatetime|System.DateTime|DateTime|DateTime|  
  
### <a name="sqlparameter-properties"></a>SqlParameter 属性  

 下表介绍了与日期和时间数据类型相关的 `SqlParameter` 属性。  
  
|属性|描述|  
|--------------|-----------------|  
|<xref:System.Data.SqlClient.SqlParameter.IsNullable%2A>|获取或设置值是否可为空。 将空参数值发送到服务器时，必须指定 <xref:System.DBNull>，而不是 `null`（在 Visual Basic 中为 `Nothing`）。 有关数据库 null 值的详细信息，请参阅 [处理 Null 值](handling-null-values.md)。|  
|<xref:System.Data.SqlClient.SqlParameter.Precision%2A>|获取或设置用于表示该值的最大位数。 对于日期和时间数据类型，此设置将被忽略。|  
|<xref:System.Data.SqlClient.SqlParameter.Scale%2A>|获取或设置要将 `Time`、`DateTime2` 和 `DateTimeOffset` 值的时间部分解析到的小数位数。 默认值为 0；也就是说，实际确定位数是根据值推断出来并发送到服务器的。|  
|<xref:System.Data.SqlClient.SqlParameter.Size%2A>|对于日期和时间数据类型，此属性将被忽略。|  
|<xref:System.Data.SqlClient.SqlParameter.Value%2A>|获取或设置参数值。|  
|<xref:System.Data.SqlClient.SqlParameter.SqlValue%2A>|获取或设置参数值。|  
  
> [!NOTE]
> 小于零或大于等于 24 小时的时间值会导致 <xref:System.ArgumentException> 抛出。  
  
### <a name="creating-parameters"></a>创建参数  

 可以通过以下方法来创建 <xref:System.Data.SqlClient.SqlParameter> 对象：使用相应的构造函数；或者通过调用 <xref:System.Data.SqlClient.SqlCommand> 的 <xref:System.Data.SqlClient.SqlCommand.Parameters%2A> 方法将其添加到 `Add`<xref:System.Data.SqlClient.SqlParameterCollection> 集合。 `Add` 方法需要使用构造函数参数或现有参数对象作为输入。  
  
 本主题接下来的几个部分举例介绍了如何指定日期和时间参数。 有关使用参数的其他示例，请参阅 [配置参数和参数数据类型](../configuring-parameters-and-parameter-data-types.md) 和 [DataAdapter 参数](../dataadapter-parameters.md)。  
  
### <a name="date-example"></a>date 示例  

 下面的代码片段展示了如何指定 `date` 参数。  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@Date";  
parameter.SqlDbType = SqlDbType.Date;  
parameter.Value = "2007/12/1";  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@Date"  
parameter.SqlDbType = SqlDbType.Date  
parameter.Value = "2007/12/1"  
```  
  
### <a name="time-example"></a>time 示例  

 下面的代码片段展示了如何指定 `time` 参数。  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@time";  
parameter.SqlDbType = SqlDbType.Time;  
parameter.Value = DateTime.Parse("23:59:59").TimeOfDay;  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@Time"  
parameter.SqlDbType = SqlDbType.Time  
parameter.Value = DateTime.Parse("23:59:59").TimeOfDay;  
```  
  
### <a name="datetime2-example"></a>datetime2 示例  

 下面的代码片段展示了如何指定同时包含日期和时间部分的 `datetime2` 参数。  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@Datetime2";  
parameter.SqlDbType = SqlDbType.DateTime2;  
parameter.Value = DateTime.Parse("1666-09-02 1:00:00");  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@Datetime2"  
parameter.SqlDbType = SqlDbType.DateTime2  
parameter.Value = DateTime.Parse("1666-09-02 1:00:00");  
```  
  
### <a name="datetimeoffset-example"></a>DateTimeOffSet 示例  

 下面的代码片段展示了如何指定包含日期、时间和时区偏移 0 的 `DateTimeOffSet` 参数。  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@DateTimeOffSet";  
parameter.SqlDbType = SqlDbType.DateTimeOffSet;  
parameter.Value = DateTimeOffset.Parse("1666-09-02 1:00:00+0");  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@DateTimeOffSet"  
parameter.SqlDbType = SqlDbType.DateTimeOffSet  
parameter.Value = DateTimeOffset.Parse("1666-09-02 1:00:00+0");  
```  
  
### <a name="addwithvalue"></a>AddWithValue  

 也可以使用 <xref:System.Data.SqlClient.SqlCommand> 的 `AddWithValue` 方法来提供参数，如下面的代码片段所示。 不过，`AddWithValue` 方法不允许为参数指定 <xref:System.Data.SqlClient.SqlParameter.DbType%2A> 或 <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A>。  
  
```csharp  
command.Parameters.AddWithValue(
    "@date", DateTimeOffset.Parse("16660902"));  
```  
  
```vb  
command.Parameters.AddWithValue( _  
    "@date", DateTimeOffset.Parse("16660902"))  
```  
  
 `@date` 参数可映射到服务器上的 `date`、`datetime` 或 `datetime2` 数据类型。 使用新 `datetime` 数据类型时，必须将参数的 <xref:System.Data.SqlDbType> 属性显式设置为实例的数据类型。 使用 <xref:System.Data.SqlDbType.Variant> 或隐式提供参数值可能会导致与 `datetime` 和 `smalldatetime` 数据类型出现向后兼容性问题。  
  
 下表展示了哪些 `SqlDbTypes` 是根据哪些 CLR 类型推断出来的：  
  
|CLR 类型|Inferred SqlDbType|  
|--------------|------------------------|  
|DateTime|SqlDbType.DateTime|  
|TimeSpan|SqlDbType.Time|  
|DateTimeOffset|SqlDbType.DateTimeOffset|  
  
## <a name="retrieving-date-and-time-data"></a>检索日期和时间数据  

 下表说明用于检索 SQL Server 2008 日期和时间值的方法。  
  
|SqlClient 方法|描述|  
|----------------------|-----------------|  
|<xref:System.Data.SqlClient.SqlDataReader.GetDateTime%2A>|以 <xref:System.DateTime> 结构形式检索指定的列值。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetDateTimeOffset%2A>|以 <xref:System.DateTimeOffset> 结构形式检索指定的列值。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificFieldType%2A>|返回作为字段的基础提供程序专用类型的类型。 对于新日期和时间类型，返回与 `GetFieldType` 相同的类型。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValue%2A>|获取指定列的值。 对于新日期和时间类型，返回与 `GetValue` 相同的类型。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValues%2A>|检索指定数组中的值。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSqlString%2A>|检索列值作为 <xref:System.Data.SqlTypes.SqlString>。 如果数据无法表示为 `SqlString`，则会导致 <xref:System.InvalidCastException> 抛出。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSqlValue%2A>|检索列数据作为默认 `SqlDbType`。 对于新日期和时间类型，返回与 `GetValue` 相同的类型。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSqlValues%2A>|检索指定数组中的值。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetString%2A>|如果 Type System Version 设置为 SQL Server 2005，则以字符串形式检索列值。 如果数据无法表示为字符串，则会导致 <xref:System.InvalidCastException> 抛出。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetTimeSpan%2A>|以 <xref:System.TimeSpan> 结构形式检索指定的列值。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetValue%2A>|检索指定列值作为基础 CLR 类型。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetValues%2A>|检索数组中的列值。|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>|返回描述结果集的元数据的 <xref:System.Data.DataTable>。|  
  
> [!NOTE]
> 正在 SQL Server 中执行的代码不支持新日期和时间 `SqlDbTypes`。 如果将其中一种类型传递给服务器，则会导致异常抛出。  
  
## <a name="specifying-date-and-time-values-as-literals"></a>作为文字指定日期和时间值  

 可以使用各种不同的文本字符串格式来指定日期和时间数据类型，随后 SQL Server 在运行时进行评估，并将它们转换为内部日期/时间结构。 SQL Server 识别用单引号 (') 括起来的日期和时间数据。 下面的示例展示了一些格式：  
  
- 字母日期格式，如 `'October 15, 2006'`。  
  
- 数字日期格式，如 `'10/15/2006'`。  
  
- 如果使用的是 ISO 标准日期格式，未分隔的字符串格式 `'20061015'`（举个例子）就会解析为 2006 年 10 月 15 日。  
  
> [!NOTE]
> SQL Server 联机丛书中有关于所有文本字符串格式以及日期和时间数据类型的其他功能的完整文档。  
  
 小于零或大于等于 24 小时的时间值会导致 <xref:System.ArgumentException> 抛出。  
  
## <a name="resources-in-sql-server-books-online"></a>SQL Server 联机丛书中的资源  

 有关使用 SQL Server 中的日期和时间值的详细信息，请参阅 SQL Server 联机丛书中的以下资源。  
  
|主题|描述|  
|-----------|-----------------|  
|[日期和时间数据类型及函数 (Transact-SQL)](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql)|概述所有 Transact-SQL 日期和时间数据类型和函数。|  
|[使用日期和时间数据](/previous-versions/sql/sql-server-2008/ms180878(v=sql.100))|提供日期和时间数据类型和函数的相关信息以及使用示例。|  
|[数据类型 (Transact-SQL)](/sql/t-sql/data-types/data-types-transact-sql)|介绍 SQL Server 中的系统数据类型。|  
  
## <a name="see-also"></a>请参阅

- [SQL Server 数据类型映射](../sql-server-data-type-mappings.md)
- [配置参数和参数数据类型](../configuring-parameters-and-parameter-data-types.md)
- [SQL Server 数据类型和 ADO.NET](sql-server-data-types.md)
- [ADO.NET 概述](../ado-net-overview.md)
