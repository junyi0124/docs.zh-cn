---
title: LINQ to Entities 中的已知问题和注意事项
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: acd71129-5ff0-4b4e-b266-c72cc0c53601
ms.openlocfilehash: 0d36461cea68383e070db80d85f596151bd9c2ae
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161953"
---
# <a name="known-issues-and-considerations-in-linq-to-entities"></a>LINQ to Entities 中的已知问题和注意事项

本部分提供有关 LINQ to Entities 查询的已知问题的信息。  
  
- [不能缓存的 LINQ 查询](#LINQQueriesThatAreNotCached)  
  
- [排序信息丢失](#OrderingInfoLost)  
  
- [不支持无符号整数](#UnsignedIntsUnsupported)  
  
- [类型转换错误](#TypeConversionErrors)  
  
- [不支持引用非标量变量](#RefNonScalarClosures)  
  
- [使用 SQL Server 2000，嵌套查询可能会失败](#NestedQueriesSQL2000)  
  
- [投影到匿名类型](#ProjectToAnonymousType)  
  
<a name="LINQQueriesThatAreNotCached"></a>

## <a name="linq-queries-that-cannot-be-cached"></a>不能缓存的 LINQ 查询  

 从 .NET Framework 4.5 开始，LINQ to Entities 查询是自动缓存的。 但是，不自动缓存将 `Enumerable.Contains` 运算符应用到内存中集合的 LINQ to Entities 查询。 此外，不允许在已编译的 LINQ 查询中参数化内存中的集合。  
  
<a name="OrderingInfoLost"></a>

## <a name="ordering-information-lost"></a>排序信息丢失  

 如果将列投影到匿名类型，则会在某些查询中丢失排序信息，这些查询将针对设置为兼容级别 "80" 的 SQL Server 2005 数据库执行。  当 order-by 列表中的列名与选择器中的列名相同时，就会发生这种情况，如下面的示例所示：  
  
 [!code-csharp[DP L2E Conceptual Examples#SBUDT543840](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#sbudt543840)]
 [!code-vb[DP L2E Conceptual Examples#SBUDT543840](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#sbudt543840)]  
  
<a name="UnsignedIntsUnsupported"></a>

## <a name="unsigned-integers-not-supported"></a>不支持无符号整数  

 不支持在 LINQ to Entities 查询中指定无符号整数类型，因为实体框架不支持无符号整数。 如果指定无符号整数，则在 <xref:System.ArgumentException> 查询表达式转换过程中会引发异常，如下面的示例中所示。 此示例查询其 ID 为 48000 的订单。  
  
 [!code-csharp[DP L2E Conceptual Examples#UIntAsQueryParam](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#uintasqueryparam)]
 [!code-vb[DP L2E Conceptual Examples#UIntAsQueryParam](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#uintasqueryparam)]  
  
<a name="TypeConversionErrors"></a>

## <a name="type-conversion-errors"></a>类型转换错误  

 在 Visual Basic 中，如果使用 `CByte` 函数将某一属性映射到值为 1 且为 SQL Server 位类型的列，则会引发 <xref:System.Data.SqlClient.SqlException> 并显示“发生算术溢出错误”消息。 下面的示例查询 AdventureWorks 示例数据库中的 `Product.MakeFlag` 列，在循环访问查询结果时引发一个异常。  
  
 [!code-vb[DP L2E Conceptual Examples#SBUDT544355](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#sbudt544355)]  
  
<a name="RefNonScalarClosures"></a>

## <a name="referencing-non-scalar-variables-not-supported"></a>不支持引用非标量变量  

 不支持在查询中引用非标量变量（如实体）。 在此类查询执行时，会引发 <xref:System.NotSupportedException> 异常，并显示以下消息：“无法创建类型为‘`EntityType`’的常量值。 此上下文中仅支持基元类型(‘如 Int32、String 和 Guid’)。”[Unable to create a constant value of type 'Closure type'. Only primitive types ('such as Int32, String, and Guid') are supported in this context.]  
  
> [!NOTE]
> 支持引用标量变量的集合。  
  
 [!code-csharp[DP L2E Conceptual Examples#SBUDT555877](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#sbudt555877)]
 [!code-vb[DP L2E Conceptual Examples#SBUDT555877](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#sbudt555877)]  
  
<a name="NestedQueriesSQL2000"></a>

## <a name="nested-queries-may-fail-with-sql-server-2000"></a>使用 SQL Server 2000，嵌套查询可能会失败  

 使用 SQL Server 2000，如果 LINQ to Entities 查询生成具有三级或以上深度的嵌套 Transact-SQL 查询，则它们可能会失败。  
  
<a name="ProjectToAnonymousType"></a>

## <a name="projecting-to-an-anonymous-type"></a>投影到匿名类型  

 如果通过使用 <xref:System.Data.Objects.ObjectQuery%601.Include%2A> 上的 <xref:System.Data.Objects.ObjectQuery%601> 方法将初始查询路径定义为包括相关对象，然后使用 LINQ 将返回的对象投影到匿名类型，则查询结果中将不包含在 Include 方法中指定的对象。  
  
 [!code-csharp[DP L2E Conceptual Examples#ProjToAnonType1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#projtoanontype1)]
 [!code-vb[DP L2E Conceptual Examples#ProjToAnonType1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#projtoanontype1)]  
  
 若要获取相关对象，请勿将返回的类型投影到匿名类型。  
  
 [!code-csharp[DP L2E Conceptual Examples#ProjToAnonType2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#projtoanontype2)]
 [!code-vb[DP L2E Conceptual Examples#ProjToAnonType2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#projtoanontype2)]  
  
## <a name="see-also"></a>请参阅

- [LINQ to Entities](linq-to-entities.md)
