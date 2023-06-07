---
title: SQL-CLR 类型映射
ms.date: 07/23/2018
ms.assetid: 4ed76327-54a7-414b-82a9-7579bfcec04b
ms.openlocfilehash: 313fd81bd84a99e4a2d32925a7d3bb4776f8472b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203535"
---
# <a name="sql-clr-type-mapping"></a>SQL-CLR 类型映射

在 LINQ to SQL 中，关系数据库的数据模型映射到用您所选择的编程语言表示的对象模型。 当应用程序运行时，LINQ to SQL 会将对象模型中的语言集成查询转换为 SQL，然后将它们发送到数据库进行执行。 当数据库返回结果时，LINQ to SQL 会将它们转换回您可以用您自己的编程语言处理的对象。  
  
 为了转换对象模型和数据库之间的数据，必须定义 *类型映射* 。 LINQ to SQL 使用类型映射将每个公共语言运行时 (CLR) 类型与一种特定 SQL Server 类型相匹配。 您可以使用基于属性的映射在对象模型内部定义类型映射和其他映射信息，例如数据库结构和表关系。 或者，您可以使用外部映射文件在对象模型外部指定映射信息。 有关详细信息，请参阅 [基于属性的映射](attribute-based-mapping.md) 和 [外部映射](external-mapping.md)。  
  
 本主题讨论下列内容：  
  
- [默认类型映射](#DefaultTypeMapping)  
  
- [类型映射运行时行为矩阵](#BehaviorMatrix)  
  
- [CLR 和 SQL 执行之间的行为差异](#BehaviorDiffs)  
  
- [枚举映射](#EnumMapping)  
  
- [数值映射](#NumericMapping)  
  
- [文本和 XML 映射](#TextMapping)  
  
- [日期和时间映射](#DateMapping)  
  
- [二进制映射](#BinaryMapping)  
  
- [杂项映射](#MiscMapping)  
  
<a name="DefaultTypeMapping"></a>

## <a name="default-type-mapping"></a>默认类型映射  

 您可以使用对象关系设计器（O/R 设计器）或 SQLMetal 命令行工具自动创建对象模型或外部映射文件。 这些工具的默认类型映射定义选择何种 CLR 类型来映射到 SQL Server 数据库内部的列。 有关使用这些工具的详细信息，请参阅 [创建对象模型](creating-the-object-model.md)。  
  
 您也可以使用 <xref:System.Data.Linq.DataContext.CreateDatabase%2A> 方法来创建基于对象模型或外部映射文件中的映射信息的 SQL Server 数据库。 <xref:System.Data.Linq.DataContext.CreateDatabase%2A> 方法的默认类型映射定义创建何种 SQL Server 列来映射到对象模型中的 CLR 类型。 有关详细信息，请参阅 [如何：动态创建数据库](how-to-dynamically-create-a-database.md)。  
  
<a name="BehaviorMatrix"></a>

## <a name="type-mapping-run-time-behavior-matrix"></a>类型映射运行时行为矩阵  

 下图显示了数据从数据库检索或保存到数据库时特定类型映射的预期运行时行为。 除序列化之外，LINQ to SQL 不支持任何该矩阵中未指定的 CLR 或 SQL Server 数据类型之间的映射。 有关序列化支持的详细信息，请参阅 [二进制序列化](#BinarySerialization)。  

![SQL Server SQL CLR 数据类型映射表](./media/sql-clr-type-mapping.png)

> [!NOTE]
> 在进行数据库转换时，某些类型映射可能会导致溢出或数据丢失异常。  
  
### <a name="custom-type-mapping"></a>自定义类型映射  

 通过使用 LINQ to SQL，您并非仅可以使用 O/R 设计器、SQLMetal 和 <xref:System.Data.Linq.DataContext.CreateDatabase%2A> 方法所使用的默认类型映射。 您可以通过在 DBML 文件中显式指定自定义类型映射来创建它们。 然后，可以使用该 DBML 文件创建对象模型代码和映射文件。 有关详细信息，请参阅 [SQL-CLR 自定义类型映射](sql-clr-custom-type-mappings.md)。  
  
<a name="BehaviorDiffs"></a>

## <a name="behavior-differences-between-clr-and-sql-execution"></a>CLR 和 SQL 执行之间的行为差异  

 由于 CLR 和 SQL Server 之间的精度及执行差异，可能会收到不同的结果或体验不同的行为，这取决于执行计算的位置。 在 LINQ to SQL 查询中执行的计算实际上转换为 Transact-SQL，然后在 SQL Server 数据库上执行。 在 LINQ to SQL 查询外执行的计算则在 CLR 的上下文中执行。  
  
 例如，以下是一些 CLR 和 SQL Server 之间的行为差异：  
  
- SQL Server 对一些数据类型的排序不同于 CLR 中等效类型数据的排序。 例如，SQL Server 类型 `UNIQUEIDENTIFIER` 的数据排序不同于 CLR 类型 <xref:System.Guid?displayProperty=nameWithType> 的数据排序。  
  
- SQL Server 处理一些字符串比较操作的方式不同于 CLR。 在 SQL Server 中，字符串比较行为取决于服务器上的排序规则设置。 有关详细信息，请参阅在 Microsoft SQL Server 联机丛书中使用 [排序规则](/previous-versions/sql/sql-server-2008-r2/ms187582(v=sql.105)) 。  
  
- 对于一些映射函数，SQL Server 返回的函数值可能与 CLR 不同。 例如，相等函数会不同，因为在两个字符串仅在尾随空白不同的情况下，SQL Server 会视这两个字符串相等，而 CLR 则视其不相等。  
  
<a name="EnumMapping"></a>

## <a name="enum-mapping"></a>枚举映射  

 LINQ to SQL 支持使用如下两种方式将 CLR <xref:System.Enum?displayProperty=nameWithType> 类型映射到 SQL Server 类型：  
  
- 映射到 SQL 数值类型（`TINYINT`、`SMALLINT`、`INT`、`BIGINT`）  
  
     将 CLR <xref:System.Enum?displayProperty=nameWithType> 类型映射到 SQL 数值类型时，您会将此 CLR <xref:System.Enum?displayProperty=nameWithType> 的基础整数值映射到 SQL Server 数据库列的值。 例如，如果一个名为 <xref:System.Enum?displayProperty=nameWithType> 的 `DaysOfWeek` 包含一个名为 `Tue` 且基础整数值为 3 的成员，则此成员映射到数据库值 3。  
  
- 映射到 SQL 文本类型（`CHAR`、`NCHAR`、`VARCHAR`、`NVARCHAR`）  
  
     将 CLR <xref:System.Enum?displayProperty=nameWithType> 类型映射到 SQL 文本类型时，SQL 数据库值会映射到 CLR <xref:System.Enum?displayProperty=nameWithType> 成员的名称。 例如，如果一个名为 <xref:System.Enum?displayProperty=nameWithType> 的 `DaysOfWeek` 包含一个名为 `Tue` 且基础整数值为 3 的成员，则此成员映射到数据库值 `Tue`。  
  
> [!NOTE]
> 将 SQL 文本类型映射到 CLR <xref:System.Enum?displayProperty=nameWithType> 时，映射的 SQL 列中仅包含 <xref:System.Enum> 成员的名称。 <xref:System.Enum> 映射的 SQL 列不支持其他值。  
  
 O/R 设计器和 SQLMetal 命令行工具无法将 SQL 类型自动映射到 CLR <xref:System.Enum> 类。 您必须通过自定义 DBML 文件以供 O/R 设计器和 SQLMetal 使用来显式配置此映射。 有关自定义类型映射的详细信息，请参阅 [SQL-CLR 自定义类型映射](sql-clr-custom-type-mappings.md)。  
  
 因为用于枚举的 SQL 列将与其他数字和文本列的类型相同;这些工具不会识别你的意图，并按以下 [数字映射](#NumericMapping) 和 [文本和 XML 映射](#TextMapping) 部分所述的方式进行映射。 有关生成带 DBML 文件的代码的详细信息，请参阅 [LINQ to SQL 中的代码生成](code-generation-in-linq-to-sql.md)。  
  
 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法创建一个数值类型的 SQL 列以映射 CLR <xref:System.Enum?displayProperty=nameWithType> 类型。  
  
<a name="NumericMapping"></a>

## <a name="numeric-mapping"></a>数值映射  

 LINQ to SQL 允许您映射多种 CLR 和 SQL Server 数值类型。 下表显示生成基于数据库的对象模型或外部映射文件时 O/R 设计器和 SQLMetal 选择的 CLR 类型。  
  
|SQL Server 类型|O/R 设计器和 SQLMetal 使用的默认 CLR 类型映射|  
|---------------------|-----------------------------------------------------------------|  
|`BIT`|<xref:System.Boolean?displayProperty=nameWithType>|  
|`TINYINT`|<xref:System.Int16?displayProperty=nameWithType>|  
|`INT`|<xref:System.Int32?displayProperty=nameWithType>|  
|`BIGINT`|<xref:System.Int64?displayProperty=nameWithType>|  
|`SMALLMONEY`|<xref:System.Decimal?displayProperty=nameWithType>|  
|`MONEY`|<xref:System.Decimal?displayProperty=nameWithType>|  
|`DECIMAL`|<xref:System.Decimal?displayProperty=nameWithType>|  
|`NUMERIC`|<xref:System.Decimal?displayProperty=nameWithType>|  
|`REAL/FLOAT(24)`|<xref:System.Single?displayProperty=nameWithType>|  
|`FLOAT/FLOAT(53)`|<xref:System.Double?displayProperty=nameWithType>|  
  
 下一个表显示 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法使用的默认类型映射，以定义创建何种 SQL 列来映射到对象模型或外部映射文件中定义的 CLR 类型。  
  
|CLR 类型|<xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 使用的默认 SQL Server 类型|  
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Boolean?displayProperty=nameWithType>|`BIT`|  
|<xref:System.Byte?displayProperty=nameWithType>|`TINYINT`|  
|<xref:System.Int16?displayProperty=nameWithType>|`SMALLINT`|  
|<xref:System.Int32?displayProperty=nameWithType>|`INT`|  
|<xref:System.Int64?displayProperty=nameWithType>|`BIGINT`|  
|<xref:System.SByte?displayProperty=nameWithType>|`SMALLINT`|  
|<xref:System.UInt16?displayProperty=nameWithType>|`INT`|  
|<xref:System.UInt32?displayProperty=nameWithType>|`BIGINT`|  
|<xref:System.UInt64?displayProperty=nameWithType>|`DECIMAL(20)`|  
|<xref:System.Decimal?displayProperty=nameWithType>|`DECIMAL(29,4)`|  
|<xref:System.Single?displayProperty=nameWithType>|`REAL`|  
|<xref:System.Double?displayProperty=nameWithType>|`FLOAT`|  
  
 有许多其他可以选择的数值映射，但是某些数值映射在转换到数据库或从数据库中转换时，可能会导致溢出或数据丢失异常。 有关详细信息，请参阅 [类型映射运行时行为矩阵](#BehaviorMatrix)。  
  
### <a name="decimal-and-money-types"></a>Decimal 和 Money 类型  

 SQL Server 类型的默认精度 `DECIMAL` (小数点左和右的18个十进制数字) 比默认配对的 CLR 类型的精度小得多 <xref:System.Decimal?displayProperty=nameWithType> 。 这可导致将数据保存到数据库时的精度降低。 但是，如果将 SQL Server `DECIMAL` 类型配置为大于 29 位精度，则会产生相反的结果。 将 SQL Server `DECIMAL` 类型的精度配置为大于 CLR <xref:System.Decimal?displayProperty=nameWithType> 时，则在从数据库检索数据时会发生精度降低。  
  
 默认情况下和 CLR `MONEY` 类型成对使用的 SQL Server `SMALLMONEY` 和 <xref:System.Decimal?displayProperty=nameWithType> 具有非常小的精度，在将数据保存到数据库时可导致溢出或数据丢失异常。  
  
<a name="TextMapping"></a>

## <a name="text-and-xml-mapping"></a>文本和 XML 映射  

 还有很多基于文本的类型和 XML 类型，您可以使用 LINQ to SQL 将其映射。 下表显示生成基于数据库的对象模型或外部映射文件时 O/R 设计器和 SQLMetal 选择的 CLR 类型。  
  
|SQL Server 类型|O/R 设计器和 SQLMetal 使用的默认 CLR 类型映射|  
|---------------------|-----------------------------------------------------------------|  
|`CHAR`|<xref:System.String?displayProperty=nameWithType>|  
|`NCHAR`|<xref:System.String?displayProperty=nameWithType>|  
|`VARCHAR`|<xref:System.String?displayProperty=nameWithType>|  
|`NVARCHAR`|<xref:System.String?displayProperty=nameWithType>|  
|`TEXT`|<xref:System.String?displayProperty=nameWithType>|  
|`NTEXT`|<xref:System.String?displayProperty=nameWithType>|  
|`XML`|<xref:System.Xml.Linq.XElement?displayProperty=nameWithType>|  
  
 下一个表显示 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法使用的默认类型映射，以定义创建何种 SQL 列来映射到对象模型或外部映射文件中定义的 CLR 类型。  
  
|CLR 类型|<xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 使用的默认 SQL Server 类型|  
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Char?displayProperty=nameWithType>|`NCHAR(1)`|  
|<xref:System.String?displayProperty=nameWithType>|`NVARCHAR(4000)`|  
|<xref:System.Char?displayProperty=nameWithType>[]|`NVARCHAR(4000)`|  
|实现 `Parse()` 和 `ToString()` 的自定义类型|`NVARCHAR(MAX)`|  
  
 有许多其他可以选择的基于文本的映射和 XML 映射，但是某些映射在转换到数据库或从数据库中转换时，可能会导致溢出或数据丢失异常。 有关详细信息，请参阅 [类型映射运行时行为矩阵](#BehaviorMatrix)。  
  
### <a name="xml-types"></a>XML 类型  

 从 Microsoft SQL Server 2005 开始，提供了 SQL Server `XML` 数据类型。 您可以将 SQL Server `XML` 数据类型映射到 <xref:System.Xml.Linq.XElement>、<xref:System.Xml.Linq.XDocument> 或 <xref:System.String>。 如果列中存储了无法读入 <xref:System.Xml.Linq.XElement> 的 XML 片段，则此列必须映射到 <xref:System.String> 以免出现运行时错误。 必须映射到 <xref:System.String> 的 XML 片段包括：  
  
- XML 元素的序列。  
  
- 特性  
  
- 公共标识符 (PI)  
  
- 注释  
  
 尽管可以 <xref:System.Xml.Linq.XElement> <xref:System.Xml.Linq.XDocument> 按 [类型映射运行时行为矩阵](#BehaviorMatrix)中所示的方式映射和到 SQL Server，但此 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法没有适用于这些类型的默认 SQL Server 类型映射。  
  
### <a name="custom-types"></a>自定义类型  

 如果类实现了 `Parse()` 和 `ToString()` ，则可以将对象映射到任何 SQL 文本类型 (`CHAR` 、 `NCHAR` 、 `VARCHAR` 、 `NVARCHAR` 、、、 `TEXT` `NTEXT` `XML`) 。 通过将 `ToString()` 返回的值发送到映射的数据库列，把对象存储在数据库中。 通过在数据库返回的字符串上调用 `Parse()` 重新构造对象。  
  
> [!NOTE]
> LINQ to SQL 不支持使用 <xref:System.Xml.Serialization.IXmlSerializable?displayProperty=nameWithType> 进行序列化。  
  
<a name="DateMapping"></a>

## <a name="date-and-time-mapping"></a>日期和时间映射  

 通过使用 LINQ to SQL，您可以映射多种 SQL Server 日期和时间类型。 下表显示生成基于数据库的对象模型或外部映射文件时 O/R 设计器和 SQLMetal 选择的 CLR 类型。  
  
|SQL Server 类型|O/R 设计器和 SQLMetal 使用的默认 CLR 类型映射|  
|---------------------|-----------------------------------------------------------------|  
|`SMALLDATETIME`|<xref:System.DateTime?displayProperty=nameWithType>|  
|`DATETIME`|<xref:System.DateTime?displayProperty=nameWithType>|  
|`DATETIME2`|<xref:System.DateTime?displayProperty=nameWithType>|  
|`DATETIMEOFFSET`|<xref:System.DateTimeOffset?displayProperty=nameWithType>|  
|`DATE`|<xref:System.DateTime?displayProperty=nameWithType>|  
|`TIME`|<xref:System.TimeSpan?displayProperty=nameWithType>|  
  
 下一个表显示 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法使用的默认类型映射，以定义创建何种 SQL 列来映射到对象模型或外部映射文件中定义的 CLR 类型。  
  
|CLR 类型|<xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 使用的默认 SQL Server 类型|  
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.DateTime?displayProperty=nameWithType>|`DATETIME`|  
|<xref:System.DateTimeOffset?displayProperty=nameWithType>|`DATETIMEOFFSET`|  
|<xref:System.TimeSpan?displayProperty=nameWithType>|`TIME`|  
  
 有许多其他可以选择的日期和时间映射，但是某些映射在转换到数据库或从数据库中转换时，可能会导致溢出或数据丢失异常。 有关详细信息，请参阅 [类型映射运行时行为矩阵](#BehaviorMatrix)。  
  
> [!NOTE]
> 从 Microsoft SQL Server 2008 开始，提供了 SQL Server 类型 `DATETIME2`、`DATETIMEOFFSET`、`DATE` 和 `TIME`。 从 .NET Framework 版本 3.5 SP1 开始，LINQ to SQL 支持映射到这些新类型。  
  
### <a name="systemdatetime"></a>System.Datetime  

 CLR <xref:System.DateTime?displayProperty=nameWithType> 类型的范围和精度大于 SQL Server `DATETIME` 类型，这是 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法的默认类型映射。 要避免与 `DATETIME` 范围之外的日期相关的异常，请使用 `DATETIME2`（从 Microsoft SQL Server 2008 开始可用）。 `DATETIME2` 可以与 CLR 的范围和精度相匹配 <xref:System.DateTime?displayProperty=nameWithType> 。  
  
 SQL Server 日期不具有 <xref:System.TimeZone>（CLR 中得到充分支持的一种功能）的概念。 无论原始 <xref:System.TimeZone> 信息如何，<xref:System.TimeZone> 值均不进行 <xref:System.DateTimeKind> 转换，按原样保存到数据库中。 从数据库中检索到 <xref:System.DateTime> 值时，它们的值按原样加载到 <xref:System.DateTime> 为 <xref:System.DateTimeKind> 的 <xref:System.DateTimeKind.Unspecified> 中。 有关支持的方法的详细信息 <xref:System.DateTime?displayProperty=nameWithType> ，请参阅 [system.web 方法](system-datetime-methods.md)。  
  
### <a name="systemtimespan"></a>System.TimeSpan  

 Microsoft SQL Server 2008 和 .NET Framework 3.5 SP1 允许您将 CLR <xref:System.TimeSpan?displayProperty=nameWithType> 类型映射到 SQL Server `TIME` 类型。 但是，CLR <xref:System.TimeSpan?displayProperty=nameWithType> 支持的范围和 SQL Server `TIME` 类型支持的范围之间存在很大的差异。 SQL `TIME` 的映射值小于 0 或大于 23:59:59.9999999 小时将导致溢出异常。 有关详细信息，请参阅 [System.web 方法](system-timespan-methods.md)。  
  
 在 Microsoft SQL Server 2000 和 SQL Server 2005 中，您无法将数据库字段映射到 <xref:System.TimeSpan>。 但是，支持对 <xref:System.TimeSpan> 的操作，原因是可以通过 <xref:System.TimeSpan> 减法运算返回 <xref:System.DateTime> 值或将这些值作为文本或绑定变量引入表达式。  
  
<a name="BinaryMapping"></a>

## <a name="binary-mapping"></a>二进制映射  

 还有很多可以映射到 CLR 类型 <xref:System.Data.Linq.Binary?displayProperty=nameWithType> 的 SQL Server 类型。 下表显示生成基于数据库的对象模型或外部映射文件时使 O/R 设计器和 SQLMetal 定义 CLR <xref:System.Data.Linq.Binary?displayProperty=nameWithType> 类型的 SQL Server 类型。  
  
|SQL Server 类型|O/R 设计器和 SQLMetal 使用的默认 CLR 类型映射|  
|---------------------|-----------------------------------------------------------------|  
|`BINARY(50)`|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|  
|`VARBINARY(50)`|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|  
|`VARBINARY(MAX)`|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|  
|`VARBINARY(MAX)` 具有 `FILESTREAM` 属性|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|  
|`IMAGE`|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|  
|`TIMESTAMP`|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|  
  
 下一个表显示 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法使用的默认类型映射，以定义创建何种 SQL 列来映射到对象模型或外部映射文件中定义的 CLR 类型。  
  
|CLR 类型|<xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 使用的默认 SQL Server 类型|  
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Data.Linq.Binary?displayProperty=nameWithType>|`VARBINARY(MAX)`|  
|<xref:System.Byte?displayProperty=nameWithType>|`VARBINARY(MAX)`|  
|<xref:System.Runtime.Serialization.ISerializable?displayProperty=nameWithType>|`VARBINARY(MAX)`|  
  
 有许多其他可以选择的二进制映射，但是某些映射在转换到数据库或从数据库中转换时，可能会导致溢出或数据丢失异常。 有关详细信息，请参阅 [类型映射运行时行为矩阵](#BehaviorMatrix)。  
  
### <a name="sql-server-filestream"></a>SQL Server FILESTREAM  

 从 Microsoft SQL Server 2008 开始，提供了 `FILESTREAM` 列的 `VARBINARY(MAX)` 属性；从 .NET Framework 版本 3.5 SP1 开始，您可以使用 LINQ to SQL 映射到该属性。  
  
 尽管您可以使用 `VARBINARY(MAX)` 属性将 `FILESTREAM` 列映射到 <xref:System.Data.Linq.Binary> 对象，但是 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法无法使用 `FILESTREAM` 属性自动创建列。 有关的详细信息 `FILESTREAM` ，请参阅 [FILESTREAM 概述](/previous-versions/sql/sql-server-2008-r2/bb933993(v=sql.105))。  
  
<a name="BinarySerialization"></a>

### <a name="binary-serialization"></a>二进制序列化  

 如果一个类实现了 <xref:System.Runtime.Serialization.ISerializable> 接口，则可以将对象序列化到任何 SQL 二进制字段 (`BINARY`、`VARBINARY`、`IMAGE`)。 将根据如何实现 <xref:System.Runtime.Serialization.ISerializable> 接口来对对象进行序列化和反序列化。 有关详细信息，请参阅 [二进制序列化](../../../../../standard/serialization/binary-serialization.md)。
  
<a name="MiscMapping"></a>

## <a name="miscellaneous-mapping"></a>杂项映射  

 下表显示一些尚未提及的杂项类型的默认类型映射。 下表显示生成基于数据库的对象模型或外部映射文件时 O/R 设计器和 SQLMetal 选择的 CLR 类型。  
  
|SQL Server 类型|O/R 设计器和 SQLMetal 使用的默认 CLR 类型映射|  
|---------------------|-----------------------------------------------------------------|  
|`UNIQUEIDENTIFIER`|<xref:System.Guid?displayProperty=nameWithType>|  
|`SQL_VARIANT`|<xref:System.Object?displayProperty=nameWithType>|  
  
 下一个表显示 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法使用的默认类型映射，以定义创建何种 SQL 列来映射到对象模型或外部映射文件中定义的 CLR 类型。  
  
|CLR 类型|<xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 使用的默认 SQL Server 类型|  
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Guid?displayProperty=nameWithType>|`UNIQUEIDENTIFIER`|  
|<xref:System.Object?displayProperty=nameWithType>|`SQL_VARIANT`|  
  
 LINQ to SQL 不支持这些杂项类型的任何其他类型映射。  有关详细信息，请参阅 [类型映射运行时行为矩阵](#BehaviorMatrix)。  
  
## <a name="see-also"></a>请参阅

- [基于特性的映射](attribute-based-mapping.md)
- [外部映射](external-mapping.md)
- [数据类型和函数](data-types-and-functions.md)
- [SQL-CLR 类型不匹配](sql-clr-type-mismatches.md)
