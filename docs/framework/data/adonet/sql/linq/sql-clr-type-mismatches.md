---
title: SQL-CLR 类型不匹配
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0a90c33f-7ed7-4501-ad5f-6224c5da8e9b
ms.openlocfilehash: 27708f4bb8e191156f578132602570bc4a6337b0
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70781198"
---
# <a name="sql-clr-type-mismatches"></a>SQL-CLR 类型不匹配

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 可以自动完成对象模型和 SQL Server 之间的大量转换。 不过，有一些情况会阻碍进行精确转换。 以下各部分将介绍公共语言运行库 (CLR) 类型与 SQL Server 数据库类型之间的主要不匹配。 可以在[SQL CLR 类型映射](sql-clr-type-mapping.md)和[数据类型和函数](data-types-and-functions.md)中找到有关特定类型映射和函数转换的更多详细信息。

## <a name="data-types"></a>数据类型

将查询发送给数据库时和将结果发送回对象模型时，CLR 和 SQL Server 之间将发生转换。 例如，下面的 Transact-SQL 查询需要进行两次值转换：

```sql
Select DateOfBirth From Customer Where CustomerId = @id
```

可在 SQL Server 上执行查询之前，必须指定 Transact-SQL 参数的值。 在此示例中，首先必须将 `id` 参数值从 CLR <xref:System.Int32?displayProperty=nameWithType> 类型转换为 SQL Server `INT` 类型，以便数据库可以理解该值。 然后，必须将 SQL Server `DateOfBirth` 列从 SQL Server `DATETIME` 类型转换为 CLR <xref:System.DateTime?displayProperty=nameWithType> 类型以在对象模型中使用，才能检索结果。 在此示例中，CLR 对象模型中的类型与 SQL Server 数据库中的类型会自然建立映射， 但不总是这样。

### <a name="missing-counterparts"></a>缺少对应项

以下类型没有合理的对应项。

- CLR <xref:System> 命名空间中的不匹配：

  - **无符号整数**。 这些类型通常映射到较大大小的有符号对应项，以避免溢出。 文本可以根据值转换为相同或较小大小的有符号数字。

  - **布尔值**。 这些类型可以映射到位、较大的数字或字符串。 文本可以映射到计算结果为相同值的表达式（例如，SQL 中的 `1=1` 对应于 CLS 中的 `True`）。

  - **TimeSpan**。 此类型表示两个 `DateTime` 值之间的差异，并且不与 SQL Server 中的 `timestamp` 相对应。 在某些情况下，CLR <xref:System.TimeSpan?displayProperty=nameWithType> 还可以映射到 SQL Server `TIME` 类型。 SQL Server `TIME` 类型只能表示小于 24 小时的正值。 而 CLR <xref:System.TimeSpan> 具有的范围则大得多。

  > [!NOTE]
  > 此比较中不包含 SQL Server <xref:System.Data.SqlTypes>特定 .NET Framework 类型。

- SQL Server 中的不匹配：

  - **固定长度字符类型**。 Transact-sql 区分 Unicode 和非 Unicode 类别，每个类别有三个不同类型：固定`nchar`长度`varchar` / `char`、可变长度`nvarchar` /和大小`ntext` /较大。 `text` 固定长度字符类型可以映射到 CLR <xref:System.Char?displayProperty=nameWithType> 类型以检索字符，但在转换和行为方面不能真正对应于同一类型。

  - **位**。 尽管 `bit` 域与 `Nullable<Boolean>` 具有相同数目的值，但二者是不同的类型。 `Bit`采用值`1`而`0`不`true`是，并且不能用作布尔表达式的等效值。/ `false`

  - **Timestamp**。 与 CLR <xref:System.TimeSpan?displayProperty=nameWithType> 类型不同，SQL Server `TIMESTAMP` 类型表示由数据库生成的 8 字节数字，它对于每次更新都是唯一的，而不是基于 <xref:System.DateTime> 值之间的差异。

  - **Money**和**SmallMoney**。 这些类型可以映射到 <xref:System.Decimal>，但本质上是不同的类型，并且基于服务器的函数和转换也将它们视为不同的类型。

### <a name="multiple-mappings"></a>多重映射

有很多可以映射到一种或多种 CLR 数据类型的 SQL Server 数据类型。 也有很多可以映射到一种或多种 SQL Server 类型的 CLR 类型。 虽然 LINQ to SQL 可能支持映射，但这并不意味着 CLR 与 SQL Server 之间映射的两种类型在精度、范围和语义上都完全匹配。 某些映射可能在以上任何或所有方面存在差异。 可以在[SQL CLR 类型映射](sql-clr-type-mapping.md)中查找各种映射可能存在的差异的详细信息。

### <a name="user-defined-types"></a>用户定义类型

用户定义的 CLR 类型旨在帮助弥合类型系统之间的差异。 不过，这些类型也引起了与类型版本管理有关的值得注意的问题。 客户端上的版本更改可能无法与数据库服务器上存储的类型的更改相匹配。 任何此类更改都会导致另一个类型不匹配，体现在其类型语义不匹配，并且版本差距可能变得明显。 在后续版本中重构继承层次结构时，会发生更多的复杂问题。

## <a name="expression-semantics"></a>表达式语义

除了 CLR 和数据库类型之间配对的不匹配之外，表达式增加了不匹配的复杂性。 必须考虑运算符语义、函数语义、隐式类型转换和优先级规则方面的不匹配。

以下小节演示了表面相似的表达式之间的不匹配。 可以生成在语义上与给定的 CLR 表达式等效的 SQL 表达式。 但是对于 CLR 用户来说，表面相似的表达式之间的语义差异是否明显，这一点并不明确。因此，是否需要进行实现语义等效方面的更改也不明确。 要计算出表达式的一组值时，这个问题尤为严重。 差异的明显程度可能取决于数据，因此很难在编码和调试过程中确定。

### <a name="null-semantics"></a>Null 语义

SQL 表达式为布尔表达式提供了三值逻辑， 结果可以是 true、false 或 null。 相比之下，CLR 为涉及 null 值的比较指定了两值布尔结果。 考虑下列代码：

[!code-csharp[DLinqMismatch#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#2)]
[!code-vb[DLinqMismatch#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#2)]

```sql
-- Assume col1 and col2 are integer columns with null values.
-- Assume that ANSI null behavior has not been explicitly
--  turned off.
Select …
From …
Where col1 = col2
-- Evaluates to null, not true and the corresponding row is not
--   selected.
-- To obtain matching behavior (i -> col1, j -> col2) change
--   the query to the following:
Select …
From …
Where
    col1 = col2
or (col1 is null and col2 is null)
-- (Visual Basic 'Nothing'.)
```

出现存在两个值的结果时会发生类似问题。

[!code-csharp[DLinqMismatch#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#3)]
[!code-vb[DLinqMismatch#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#3)]

```sql
-- Assume col1 and col2 are nullable columns.
-- Assume that ANSI null behavior has not been explicitly
--   turned off.
Select …
From …
Where
    col1 = col2
or col1 != col2
-- Visual Basic: col1 <> col2.

-- Excludes the case where the boolean expression evaluates
--   to null. Therefore the where clause does not always
--   evaluate to true.
```

在上例中，在生成 SQL 方面可能得出等效的行为，但是转换可能不会准确地反映您的意图。

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]不会对C# `null` SQL 施加`nothing`或 Visual Basic 比较语义。 比较运算符在语法上被转换为其 SQL 等效项。 反映 SQL 语义的语义是由服务器或连接设置定义的。 在默认的 SQL Server 设置下，两个 null 值被视为不相等（尽管您可以更改设置以改变语义）。 无论如何，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 在查询转换中不会考虑服务器设置。

带有文本 `null` (`nothing`) 的比较被转换为相应的 SQL 版本（`is null` 或 `is not null`）。

排序规则中的 `null` (`nothing`) 值是由 SQL Server 定义的；[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不会更改排序规则。

### <a name="type-conversion-and-promotion"></a>类型转换和提升

SQL 支持在表达式中使用一组丰富的隐式转换。 C# 中的类似表达式则需要显式强制转换。 例如:

- `Nvarchar` 和 `DateTime` 类型在 SQL 中可以不经过任何显式强制转换进行比较，而在 C# 中则需要显式转换。

- `Decimal` 在 SQL 中隐式转换为 `DateTime`。 C# 不允许使用隐式转换。

同样，由于基础类型集不同，Transact-SQL 中的类型优先级与 C# 中的类型优先级不同。 实际上，在优先级列表之间没有明确的子集/父集关系。 例如，将 `nvarchar` 和 `varchar` 进行比较会导致 `varchar` 表达式隐式转换为 `nvarchar`。 CLR 不提供等效的提升。

在简单的情况下，这些差异会导致 CLR 表达式包含对于相应的 SQL 表达式来说多余的强制转换。 更重要的是，SQL 表达式的中间结果可能会隐式提升到在 C# 中没有精确对应项的类型，反之亦然。 总之，此类表达式的测试、调试和验证会给用户增加大量的负担。

### <a name="collation"></a>排序规则

Transact-SQL 支持作为字符串类型的批注的显式排序规则。 这些排序规则决定了某些比较的有效性。 例如，比较两个使用不同显式排序规则的列将会出错。 使用大大简化的 CTS 字符串类型不会导致这种错误。 请看下面的示例：

```sql
create table T2 (
    Col1 nvarchar(10),
    Col2      nvarchar(10) collate Latin_general_ci_as
)
```

[!code-csharp[DLinqMismatch#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#4)]
[!code-vb[DLinqMismatch#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#4)]

```sql
Select …
From …
Where Col1 = Col2
-- Error, collation conflict.
```

实际上，排序规则子句会创建不可替换的*受限类型*。

同样，各个类型系统的排序顺序会有明显差异。 这种差异会影响到结果排序。 <xref:System.Guid> 对全部 16 个字节按字典顺序排序 (`IComparable()`)，而 T-SQL 按下面的顺序比较 GUID：node(10-15)、clock-seq(8-9)、time-high(6-7)、time-mid(4-5)、time-low(0-3)。 当 NT 生成的 GUID 具有类似的八位字节顺序时，此排序在 SQL 7.0 中完成。 该方法确保在同一节点群集上生成的 GUID 按时间戳的顺序集合。 该方法还可用于生成索引（插入改为追加而不是随机 IO）。 出于保密考虑，该顺序稍后在 Windows 中加密，但 SQL 必须维护兼容性。 解决方法是使用<xref:System.Data.SqlTypes.SqlGuid> <xref:System.Guid>而不是。

### <a name="operator-and-function-differences"></a>运算符和函数差异

本质上可比较的运算符和函数在语义上稍有不同。 例如:

- C# 基于逻辑运算符 `&&` 和 `||` 的操作数的词法顺序指定短路语义。 另一方面，SQL 面向基于集的查询，因此在决定执行顺序方面为优化器提供了更大的自由度。 由此产生的一些连带影响包括：

  - 语义等效转换需要 "`CASE` 。 `WHEN` … `THEN`"在 SQL 中构造，以避免对操作数执行重新排序。

  - 如果C#表达式依赖于`AND`根据第一个操作数的计算结果来计算第二个操作数，则对运算符进行/ `OR`松散转换可能导致意外错误。

- `Round()`函数在 .NET Framework 和 T-sql 中具有不同的语义。

- 字符串的起始索引在 CLR 中是 0，而在 SQL 中是 1。 因此，任何具有索引的函数都需要进行索引转换。

- CLR 支持用于浮点数的模数 (‘%’) 运算符，但 SQL 不支持。

- `Like` 运算符基于隐式转换有效地获取自动重载。 虽然 `Like` 运算符被定义为对字符串类型发生作用，但如果对数字类型或 `DateTime` 类型进行隐式转换，则 `Like` 也可以用于这些非字符串类型。 在 CTS 中，不存在相应的隐式转换。 因此，需要其他重载。

    > [!NOTE]
    > 此 `Like` 运算符行为仅适用于 C#；Visual Basic `Like` 关键字保持不变。

- 在 SQL 中始终检查溢出，但必须在（而不是C# Visual Basic）中显式指定溢出才能避免环绕。 假设有整数列 C1、C2 和 C3，并且 C1+C2 存储在 C3 中 (Update T Set C3 = C1 + C2)。

    ```sql
    create table T3 (
        Col1      integer,
        Col2      integer
    )
    insert into T3 (col1, col2) values (2147483647, 5)
    -- Valid values: max integer value and 5.
    select * from T3 where col1 + col2 < 0
    -- Produces arithmetic overflow error.
    ```

[!code-csharp[DLinqMismatch#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#5)]
[!code-vb[DLinqMismatch#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#5)]

- 当 .NET Framework 使用银行家舍入时，SQL 执行对称算术舍入。 有关更多信息，请参见知识库文章 196652。

- 默认情况下，对于通用区域设置，字符串比较在 SQL 中不区分大小写。 在 Visual Basic 和 C# 中，它们区分大小写。 例如，如果`s == "Food"` `s == "Food"` `s = "Food"` 为，`food`则（在 Visual Basic 中）和可能会产生不同的结果。 `s`

    ```sql
    -- Assume default US-English locale (case insensitive).
    create table T4 (
        Col1      nvarchar (256)
    )
    insert into T4 values (‘Food’)
    insert into T4 values (‘FOOD’)
    select * from T4 where Col1 = ‘food’
    -- Both the rows are returned because of case-insensitive matching.
    ```

[!code-csharp[DLinqMismatch#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#6)]
[!code-vb[DLinqMismatch#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#6)]

- 在语义上，SQL 中适用于固定长度字符类型参数的运算符/函数与适用于 CLR <xref:System.String?displayProperty=nameWithType> 的相同运算符/函数有明显不同。 这也可以看作是关于类型的小节中讨论的缺少对应项问题的延伸。

    ```sql
    create table T4 (
        Col1      nchar(4)
    )
    Insert into T5(Col1) values ('21');
    Insert into T5(Col1) values ('1021');
    Select * from T5 where Col1 like '%1'
    -- Only the second row with Col1 = '1021' is returned.
    -- Not the first row!
    ```

     [!code-csharp[DLinqMismatch#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#7)]
     [!code-vb[DLinqMismatch#7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#7)]

     字符串串联存在类似的问题。

    ```sql
    create table T6 (
        Col1      nchar(4)
        Col2       nchar(4)
    )
    Insert into T6 values ('a', 'b');
    Select Col1+Col2 from T6
    -- Returns concatenation of padded strings "a   b   " and not "ab".
    ```

总之，可能需要对 CLR 表达式进行复杂转换，同时可能需要附加的运算符/函数来公开 SQL 功能。

### <a name="type-casting"></a>类型强制转换

在 C# 和 SQL 中，用户可以使用显式类型强制转换（`Cast` 和 `Convert`）来重写默认的表达式语义。 但是，跨类型系统边界公开此功能将带来一个难题。 提供所需语义的 SQL 强制转换不能方便地转换为相应的 C# 强制转换。 另一方面，由于类型不匹配、缺少对应项和类型优先级层次结构不同，C# 强制转换无法直接转换为等效的 SQL 强制转换。 需要在出现类型系统不匹配与损失表达式强大功能之间进行权衡。

在其他情况下，可能不需要对任何一种验证表达式的情况使用类型强制转换，但可能需要使用它来确保非默认映射正确地应用到表达式。

```sql
-- Example from "Non-default Mapping" section extended
create table T5 (
    Col1      nvarchar(10),
    Col2      nvarchar(10)
)
Insert into T5(col1, col2) values (‘3’, ‘2’);
```

[!code-csharp[DLinqMismatch#8](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#8)]
[!code-vb[DLinqMismatch#8](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#8)]

```sql
Select *
From T5
Where Col1 + Col2 > 4
-- "Col1 + Col2" expr evaluates to '32'
```

## <a name="performance-issues"></a>性能问题

对于某些 SQL Server，CLR 类型的差异可能导致在 CLR 与 SQL Server 类型系统之间相交时性能下降。 影响性能的方案示例包括：

- 强制对逻辑“与”/逻辑“或”运算符的计算顺序

- 生成 SQL 以对谓词计算执行排序会限制 SQL 优化器的功能。

- 类型转换（无论是由 CLR 编译器引入还是由对象关系查询实现引入）可能会限制索引的使用。

     例如，应用于对象的

    ```sql
    -- Table DDL
    create table T5 (
        Col1      varchar(100)
    )
    ```

     [!code-csharp[DLinqMismatch#9](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqMismatch/cs/Program.cs#9)]
     [!code-vb[DLinqMismatch#9](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqMismatch/vb/Module1.vb#9)]

     请考虑表达式 `(s = SOME_STRING_CONSTANT)` 的转换。

    ```sql
    -- Corresponding part of SQL where clause
    Where …
    Col1 = SOME_STRING_CONSTANT
    -- This expression is of the form <varchar> = <nvarchar>.
    -- Hence SQL introduces a conversion from varchar to nvarchar,
    --   resulting in
    Where …
    Convert(nvarchar(100), Col1) = SOME_STRING_CONSTANT
    -- Cannot use the index for column Col1 for some implementations.
    ```

除了语义差异，考虑在 SQL Server 与 CLR 类型系统之间切换时对性能的影响非常重要。 对于大型数据集，此类性能问题可能决定应用程序是否可部署。

## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
