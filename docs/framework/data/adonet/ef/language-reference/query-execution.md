---
title: 查询执行
description: 了解 LINQ to Entities 查询运行的不同方法，包括延迟的查询执行、即时查询执行和存储执行。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c0e6cf23-63ac-47dd-bfe9-d5bdca826fac
ms.openlocfilehash: e5961330eab5f25508319f276df1e9b4572f49ee
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189300"
---
# <a name="query-execution"></a>查询执行

在用户创建 LINQ 查询后，该查询将转换为一个命令目录树。 命令目录树是与实体框架兼容的查询表示形式。 然后，针对数据源执行该命令目录树。 在执行查询时，将计算所有查询表达式（即查询的所有组成部分），包括在结果具体化中使用的那些表达式。  
  
 执行查询表达式的时间点可能会有所不同。 LINQ 查询始终在循环访问查询变量时执行，而不是在创建查询变量时执行。 这称为 " *延迟执行*"。 您也可以强制立即执行查询，这对于缓存查询结果很有用。 本主题稍后将对此进行介绍。  
  
 当执行 LINQ to Entities 查询时，查询中的有些表达式可能在服务器上执行，而有些部分可能在客户端上本地执行。 表达式的客户端计算发生于在服务器上执行查询之前。 如果在客户端上计算表达式，则该计算的结果将替换查询中的表达式，然后在服务器上执行查询。 由于是在数据源上执行查询，因此数据源配置将覆盖客户端中指定的行为。 例如，null 值处理和数值精度取决于服务器设置。 在查询执行期间在服务器上引发的任何异常都将直接向上传递到客户端。  

> [!TIP]
> 若要获取表格式的查询运算符（可用于快速标识运算符的执行行为）的简单摘要，请参阅 [标准查询运算符分类通过执行 (c # ) 的方式 ](../../../../../csharp/programming-guide/concepts/linq/classification-of-standard-query-operators-by-manner-of-execution.md)。

## <a name="deferred-query-execution"></a>延迟执行查询  

 在返回一系列值的查询中，查询变量本身从不保存查询结果，它只存储查询命令。 查询的执行将推迟到在 `foreach` 或 `For Each` 循环中循环访问查询变量之后进行。 这称为 " *延迟执行*";也就是说，在构造查询后，将执行查询。 这意味着您可以根据需要频繁地执行查询。 例如，当您的数据库由其他应用程序不断更新时，此功能将会很有用。 在您的应用程序中，您可以创建查询以检索最新信息并重复执行查询，每次返回更新的信息。  
  
 延迟执行可使多个查询组合在一起或使查询得到扩展。 扩展查询时，将修改查询以包括新操作，最终执行将反映这些更改。 在下面的示例中，第一个查询返回所有产品。 第二个查询通过使用 `Where` 扩展第一个查询，以返回大小为“L”的所有产品：  
  
 [!code-csharp[DP L2E Conceptual Examples#Composing1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#composing1)]
 [!code-vb[DP L2E Conceptual Examples#Composing1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#composing1)]  
  
 在执行查询后，所有后续查询都将使用内存中的 LINQ 运算符。 通过使用 `foreach` 或 `For Each` 语句或者调用某个 LINQ 转换运算符来循环访问查询变量会使查询立即执行。 这些转换运算符包括：<xref:System.Linq.Enumerable.ToList%2A>、<xref:System.Linq.Enumerable.ToArray%2A>、<xref:System.Linq.Enumerable.ToLookup%2A> 和 <xref:System.Linq.Enumerable.ToDictionary%2A>。  
  
## <a name="immediate-query-execution"></a>立即执行查询  

 与返回一系列值的延迟执行查询相反，返回单一实例值的查询将立即执行。 <xref:System.Linq.Enumerable.Average%2A>、<xref:System.Linq.Enumerable.Count%2A>、<xref:System.Linq.Enumerable.First%2A> 和 <xref:System.Linq.Enumerable.Max%2A> 是单一实例查询的一些示例。 这些查询立即执行，因为查询必须生成序列才能计算单一实例结果。 您也可以强制立即执行。 如果要缓存查询结果，这么做十分有用。 若要强制立即执行不生成单一实例值的查询，可以对查询或查询变量调用 <xref:System.Linq.Enumerable.ToList%2A>、<xref:System.Linq.Enumerable.ToDictionary%2A> 或 <xref:System.Linq.Enumerable.ToArray%2A> 方法。 以下示例使用 <xref:System.Linq.Enumerable.ToArray%2A> 方法以立即将序列转换为数组。  
  
 [!code-csharp[DP L2E Examples#ToArray](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#toarray)]
 [!code-vb[DP L2E Examples#ToArray](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#toarray)]  
  
 通过在查询表达式之后紧随 `foreach` 或 `For Each` 循环，也可以强制执行，而通过调用 <xref:System.Linq.Enumerable.ToList%2A> 或 <xref:System.Linq.Enumerable.ToArray%2A>，可以缓存单个集合对象中的所有数据。  
  
## <a name="store-execution"></a>存储区执行  

 通常，LINQ to Entities 中的表达式将在服务器上计算，表达式的行为不会遵循公共语言运行库 (CLR) 语义，而是遵循数据源的语义。 但存在一些例外情况，例如，在客户端上执行表达式时。 这可能会导致意外结果，例如，服务器和客户端位于不同的时区时。  
  
 查询中的某些表达式可能在客户端上执行。 通常，大多数查询表达式应在服务器上执行。 除了对映射到数据源的查询元素所执行的方法之外，查询中通常还有一些表达式可在本地执行。 本地执行查询表达式会生成一个可在查询执行或结果构造中使用的值。  
  
 某些操作始终在客户端上执行，例如，闭包中值、子表达式、子查询的绑定以及将对象具体化到查询结果等。 这种做法的最终结果是，这些元素（如参数值）不能在执行期间更新。 匿名类型可以在数据源上以内联方式构造，但不应假定可以这样做。 也可以在数据源中构造内联分组，但不应假定在所有情况下都可以这样做。 总之，最好不要对于哪些内容是在服务器上构造的进行假定。  
  
 本节介绍在客户端本地执行代码的方案。 有关本地执行的表达式类型的详细信息，请参阅 [LINQ to Entities 查询中的表达式](expressions-in-linq-to-entities-queries.md)。  
  
### <a name="literals-and-parameters"></a>文本和参数  

 本地变量（如下例中的 `orderID` 变量）是在客户端上计算的。  
  
 [!code-csharp[DP L2E Conceptual Examples#LiteralParameter1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#literalparameter1)]
 [!code-vb[DP L2E Conceptual Examples#LiteralParameter1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#literalparameter1)]  
  
 方法参数也是在客户端上计算的。 传入以下 `orderID` 方法的 `MethodParameterExample` 参数就是一个示例。  
  
 [!code-csharp[DP L2E Conceptual Examples#MethodParameterExample](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#methodparameterexample)]
 [!code-vb[DP L2E Conceptual Examples#MethodParameterExample](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#methodparameterexample)]  
  
### <a name="casting-literals-on-the-client"></a>在客户端上强制转换文本  

 在客户端上执行从 `null` 到 CLR 类型的强制转换：  
  
 [!code-csharp[DP L2E Conceptual Examples#NullCastToString](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#nullcasttostring)]
 [!code-vb[DP L2E Conceptual Examples#NullCastToString](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#nullcasttostring)]  
  
 在客户端上执行到某一类型（如可以为 null 的 <xref:System.Decimal>）的强制转换：  
  
 [!code-csharp[DP L2E Conceptual Examples#CastToNullable](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#casttonullable)]
 [!code-vb[DP L2E Conceptual Examples#CastToNullable](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#casttonullable)]  
  
### <a name="constructors-for-literals"></a>用于文本的构造函数  

 可映射到概念模型类型的新 CLR 类型是在客户端上执行的：  
  
 [!code-csharp[DP L2E Conceptual Examples#ConstructorForLiteral](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#constructorforliteral)]
 [!code-vb[DP L2E Conceptual Examples#ConstructorForLiteral](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#constructorforliteral)]  
  
 新数组也是在客户端上执行的。  
  
## <a name="store-exceptions"></a>存储区异常  

 执行查询时出现的任何存储区错误都会向上传递到客户端，不会进行映射或处理。  
  
## <a name="store-configuration"></a>存储区配置  

 在存储区上执行查询时，存储区配置将覆盖所有客户端行为，并用存储区语义表示所有运算和表达式。 这可能导致 CLR 与存储区执行之间的行为在某些方面存在差异，例如 null 比较、GUID 排序、涉及非精确数据类型（如浮点类型或 <xref:System.DateTime>）运算的准确性以及字符串运算等。 检查查询结果时，请务必注意这一点。  
  
 例如，以下是一些 CLR 和 SQL Server 之间的行为差异：  
  
- 在 GUID 排序方式上，SQL Server 不同于 CLR。  
  
- 在处理 SQL Server 十进制类型时，这两者在结果精度方面也存在差异。 这是因为 SQL Server 十进制类型具有固定精度要求。 例如，在客户端内存中，<xref:System.Decimal> 值 0.0、0.0 和 1.0 的平均值为 0.3333333333333333333333333333，而在存储区中则为 0.333333（基于 SQL Server 的十进制类型的默认精度）。  
  
- 在处理一些字符串比较运算的方式上，SQL Server 也不同于 CLR。 在服务器上，字符串比较行为取决于排序规则设置。  
  
- 函数或方法调用如果包含在 LINQ to Entities 查询中，则会先映射到实体框架中的规范函数，然后再转换为 Transact-SQL 并针对 SQL Server 数据库执行。 在有些情况下，这些映射函数的行为可能不同于基类库中的实现。 例如，将一个空字符串用作参数调用 <xref:System.String.Contains%2A>、<xref:System.String.StartsWith%2A> 和 <xref:System.String.EndsWith%2A> 方法时，如果在 CLR 中执行，会返回 `true`；如果在 SQL Server 中执行，则会返回 `false`。 如果两个字符串只有尾随空格不同，SQL Server 认为它们相等，而 CLR 认为它们不相等，因此 <xref:System.String.EndsWith%2A> 方法也会返回不同的结果。 下面的示例对此进行演示：  
  
 [!code-csharp[DP L2E Conceptual Examples#CanonicalFuncVsCLRBaseType](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#canonicalfuncvsclrbasetype)]
 [!code-vb[DP L2E Conceptual Examples#CanonicalFuncVsCLRBaseType](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#canonicalfuncvsclrbasetype)]
