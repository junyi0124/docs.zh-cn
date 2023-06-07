---
title: 公共架构集合
ms.date: 03/30/2017
ms.assetid: 50127ced-2ac8-4d7a-9cd1-5c98c655ff03
ms.openlocfilehash: f822de27e53554aba4011a701f59a8feda847c67
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91203808"
---
# <a name="common-schema-collections"></a>公共架构集合

通用架构集合是每个 .NET Framework 托管提供程序均实现的架构集合。 可以通过调用不带任何参数的 **GetSchema** 方法或使用架构集合名称 "MetaDataCollections"，查询 .NET Framework 托管提供程序以确定支持的架构集合列表。 此时将返回 <xref:System.Data.DataTable>，包含支持的架构集合列表、每个架构集合支持的限制数以及所使用的标识符部分数。 这些集合描述所有必需的列。 提供程序可以根据需要随意添加其他列。 例如，`SqlClient` 和 `OracleClient` 向限制集合中添加 ParameterName。  
  
 如果提供程序无法确定所需列的值，将返回空值。  
  
 有关使用 **GetSchema** 方法的详细信息，请参阅 [GetSchema 和架构集合](getschema-and-schema-collections.md)。  
  
## <a name="metadatacollections"></a>MetaDataCollections  

 此架构集合为当前用于连接到数据库上的 .NET Framework 管理的应用程序公开所支持的所有架构集合的有关信息。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|CollectionName|字符串|要传递给 **GetSchema** 方法以返回集合的集合的名称。|  
|NumberOfRestrictions|int|可以为集合指定的限制数。|  
|NumberOfIdentifierParts|int|复合标识符/数据库对象名称中的部分数。 例如，在 SQL Server 中，表为 3，列为 4。 在 Oracle 中，表为 2，列为 3。|  
  
## <a name="datasourceinformation"></a>DataSourceInformation  

 此架构集合为 .NET Framework 管理的提供程序当前连接到的数据源公开有关信息。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|CompositeIdentifierSeparatorPattern|字符串|匹配复合标识符中的复合分隔符的正则表达式。 例如“\\”。 SQL Server 的 () 或 " \@&#124;" \\ 。 （针对 Oracle）。<br /><br /> 复合标识符通常用于数据库对象名称，例如： pubs. dbo. 作者或 pubs \@ dbo。<br /><br /> 对于 SQL Server，请使用正则表达式 " \\ ."。 对于 System.data.oracleclient，请使用 " \@&#124;\\ 。"。<br /><br /> 对于 ODBC，使用 Catalog_name_seperator。<br /><br /> 对于 OLE DB，使用 DBLITERAL_CATALOG_SEPARATOR 或 DBLITERAL_SCHEMA_SEPARATOR。|  
|DataSourceProductName|字符串|通过提供程序访问的产品名称，例如“Oracle”或“SQLServer”。|  
|DataSourceProductVersion|字符串|指示提供程序访问的产品版本，采用数据源本机格式，而不是 Microsoft 格式。<br /><br /> 有时，DataSourceProductVersion 和 DataSourceProductVersionNormalized 的值相同。 对于 OLE DB 和 ODBC，这两个值始终相同，因为它们映射到基础本机 API 中相同的函数调用。|  
|DataSourceProductVersionNormalized|字符串|数据源的标准化版本，以便可以使用 `String.Compare()` 进行比较。 对于提供程序的所有版本，此值的格式一致，以避免版本 10 排序在版本 1 和版本 2 之间。<br /><br /> 例如，Oracle 提供程序对其标准化版本使用 "nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn. nn SQL Server 使用典型的 Microsoft "nn. nn。<br /><br /> 有时，DataSourceProductVersion 和 DataSourceProductVersionNormalized 的值相同。 对于 OLE DB 和 ODBC，这两个值始终相同，因为它们映射到基础本机 API 中相同的函数调用。|  
|GroupByBehavior|<xref:System.Data.Common.GroupByBehavior>|指定 GROUP BY 子句中的列与选择列表中的非聚合列之间的关系。|  
|IdentifierPattern|字符串|匹配标识符并且包含标识符的匹配值的正则表达式。 例如“[A-Za-z0-9_#$]”。|  
|IdentifierCase|<xref:System.Data.Common.IdentifierCase>|表示未加引号的标识符是否区分大小写。|  
|OrderByColumnsInSelect|bool|指定 ORDER BY 子句中的列是否必须在选择列表中。 如果值为 true，表示这些列必须在选择列表中，如果值为 false，表示这些列不必在选择列表中。|  
|ParameterMarkerFormat|字符串|表示如何格式化参数的格式化字符串。<br /><br /> 如果数据源不支持命名的参数，此字符串中的第一个占位符应是格式化参数名的位置。<br /><br /> 例如，如果数据源需要命名参数并以 "：" 作为前缀，则这将是 "： {0} "。 在使用参数名“p1”格式化此字符串时，生成的字符串为“:p1”。<br /><br /> 如果数据源期望参数以 "" 作为前缀 \@ ，但名称中已包含这些参数，则这将是 " {0} "，而格式为 "p1" 的参数的结果 \@ 将只是 " \@ p1"。<br /><br /> 对于不应使用命名参数并希望使用 "？" 字符的数据源，可以将格式字符串指定为 "？"，这将忽略参数名。 对于 OLE DB 我们返回 "？"。|  
|ParameterMarkerPattern|字符串|匹配参数标记的正则表达式。 该表达式将包含参数名的匹配值（如果有）。<br /><br /> 例如，如果支持命名参数，且 \@ 参数名称中包含 "" 前导字符，这将是： " (\@ [a-za-z0-9_ $ #] * ) "。<br /><br /> 但是，如果支持命名参数时使用 "：" 作为前导字符，并且它不是参数名称的一部分，则为： "： ( [a-za-z0-9_ $ #] \*) "。<br /><br /> 当然，如果数据源不支持命名参数，则这只是 "？"。|  
|ParameterNameMaxLength|int|参数名的最大长度（字符数）。 Visual Studio 期望如果支持参数名，最大长度的最小值为 30 个字符。<br /><br /> 如果数据源不支持命名参数，此属性将返回零。|  
|ParameterNamePattern|字符串|匹配有效参数名的正则表达式。 对于参数名可以使用的字符，不同的数据源采用不同的规则。<br /><br /> Visual Studio 期望如果支持参数名，字符“\p{Lu}\p{Ll}\p{Lt}\p{Lm}\p{Lo}\p{Nl}\p{Nd}”是参数名支持的最小有效字符集。|  
|QuotedIdentifierPattern|字符串|匹配加引号的标识符并且包含标识符本身（不加引号）的匹配值的正则表达式。 例如，如果数据源使用双引号标识带引号的标识符，则为： " ( # A1 [^ \\ "] &#124;\\ " \\ " ) * ) "。|  
|QuotedIdentifierCase|<xref:System.Data.Common.IdentifierCase>|表示加引号的标识符是否区分大小写。|  
|StatementSeparatorPattern|字符串|匹配语句分隔符的正则表达式。|  
|StringLiteralPattern|字符串|匹配字符串文本并且包含文本本身的匹配值的正则表达式。 例如，如果数据源使用单引号标识字符串，则这是： " (" ( [^ "] &#124;" ") *" ) ""|  
|SupportedJoinOperators|<xref:System.Data.Common.SupportedJoinOperators>|指定数据源支持的 SQL join 语句类型。|  
  
## <a name="datatypes"></a>DataTypes  

 此架构集合为 .NET Framework 管理的提供程序当前连接到的数据库公开所支持的数据类型的有关信息。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|TypeName|字符串|提供程序特定的数据类型名称。|  
|ProviderDbType|int|指定参数类型时应使用的特定于提供程序的类型值。 例如 SqlDbType.Money 或 OracleType.Blob。|  
|ColumnSize|long|非数值列或参数的长度是指提供程序为此类型定义的最大值或长度。<br /><br /> 对于字符数据，此值是指最大值或定义长度，单位由数据源定义。 对于某些字符数据类型，Oracle 的观念是先指定长度，然后指定实际存储大小。 对于 Oracle，此值仅定义长度（单位）。<br /><br /> 对于日期时间数据类型，此值是指字符串表示形式的长度（假定小数秒元素允许的最大精度）。<br /><br /> 如果数据类型为数值，则是数据类型的最大精度的上限。|  
|CreateFormat|字符串|格式化字符串，表示如何将此列添加到数据定义语句中，例如 CREATE TABLE。 CreateParameter 数组中的每个元素应通过格式字符串中的“参数标记”表示。<br /><br /> 例如，SQL 数据类型 DECIMAL 需要精度和小数位数。 在这种情况下，格式字符串将为 "DECIMAL ({0} ， {1}) "。|  
|CreateParameters|字符串|创建此数据类型的列时必须指定的创建参数。 每个创建参数按照要提供的顺序在该字符串中列出，使用逗号分隔。<br /><br /> 例如，SQL 数据类型 DECIMAL 需要精度和小数位数。 在此情况下，创建参数应包含字符串“精度, 小数位数”。<br /><br /> 在用于创建精度为10、小数位数为2的 DECIMAL 列的文本命令中，CreateFormat 列的值可能为 DECIMAL ({0} ， {1}) "，完整类型规范为 decimal (10，2) 。|  
|数据类型|字符串|数据类型的 .NET Framework 类型的名称。|  
|IsAutoincrementable|bool|true — 此数据类型的值可能是自动递增的。<br /><br /> false — 此数据类型的值可能不是自动递增的。<br /><br /> 注意，这只是表示此数据类型的列可能是自动递增的，而不是表示所有此类型的列都是自动递增的。|  
|IsBestMatch|bool|true — 数据类型是数据存储中的所有数据类型与 DataType 列中的值指示的 .NET Framework 数据类型之间的最佳匹配。<br /><br /> false — 数据类型不是最佳匹配。<br /><br /> 对于 DataType 列的值相同的每组行，IsBestMatch 列只在一行中设置为 true。|  
|IsCaseSensitive|bool|true — 数据类型是字符类型，并且区分大小写。<br /><br /> false — 数据类型不是字符类型，或不区分大小写。|  
|IsFixedLength|bool|true — 通过数据定义语言 (DDL) 创建的此数据类型的列将为固定长度。<br /><br /> false — 通过 DDL 创建的此数据类型的列将为可变长度。<br /><br /> DBNull.Value — 不知道提供程序将此字段与固定长度列还是可变程度列映射。|  
|IsFixedPrecisionScale|bool|true — 数据类型具有固定的精度和小数位数。<br /><br /> false — 数据类型没有固定的精度和小数位数。|  
|IsLong|bool|true — 数据类型包含的数据很长；很长数据的定义是提供程序特定的。<br /><br /> false — 数据类型未包含很长的数据。|  
|IsNullable|bool|true — 数据类型可为空。<br /><br /> false — 数据类型不可为空。<br /><br /> DBNull.Value — 不知道数据类型是否可为空。|  
|IsSearchable|bool|true — 数据类型可以在包含除 LIKE 谓词以外的任何运算符的 WHERE 子句中使用。<br /><br /> false — 数据类型不能在包含除 LIKE 谓词以外的任何运算符的 WHERE 子句中使用。|  
|IsSearchableWithLike|bool|true — 数据类型可以与 LIKE 谓词一起使用。<br /><br /> false — 数据类型不能与 LIKE 谓词一起使用。|  
|IsUnsigned|bool|true — 数据类型无符号。<br /><br /> false — 数据类型有符号。<br /><br /> DBNull.Value — 不适用于数据类型。|  
|MaximumScale|short|如果类型指示符为数值类型，是指小数点右侧允许的最大位数。 否则为 DBNull.Value。|  
|MinimumScale|short|如果类型指示符为数值类型，是指小数点右侧允许的最小位数。 否则为 DBNull.Value。|  
|IsConcurrencyType|bool|true – 每次行更改并且列值与所有以前的值不同时，由数据库更新数据类型。<br /><br /> false – 每次行更改时，数据库不更新数据类型。<br /><br /> DBNull.Value – 数据库不支持此数据类型的类型。|  
|IsLiteralSupported|bool|true – 数据类型可以以文本形式表示。<br /><br /> false – 数据类型不能以文本形式表示。|  
|LiteralPrefix|字符串|应用于给定文本的前缀。|  
|LiteralSuffix|字符串|应用于给定文本的后缀。|  
|NativeDataType|字符串|NativeDataType 是 OLE DB 特定的列，用于公开数据类型的 OLE DB 类型。|  
  
## <a name="restrictions"></a>限制  

 此架构集合为当前用于连接到数据库的 .NET Framework 管理的提供程序公开所支持的限制的有关信息。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|CollectionName|字符串|应用这些限制的集合的名称。|  
|RestrictionName|字符串|集合中的限制的名称。|  
|RestrictionDefault|字符串|已忽略。|  
|RestrictionNumber|int|此特定限制在限制集合中的实际位置。|  
  
## <a name="reservedwords"></a>ReservedWords  

 此架构集合为 .NET Framework 管理的提供程序当前连接到的数据库公开所保留的关键字的有关信息。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|ReservedWord|字符串|提供程序特定的保留字。|  
  
## <a name="see-also"></a>另请参阅

- [检索数据库架构信息](retrieving-database-schema-information.md)
- [GetSchema 和架构集合](getschema-and-schema-collections.md)
- [ADO.NET 概述](ado-net-overview.md)
