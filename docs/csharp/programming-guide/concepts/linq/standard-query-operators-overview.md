---
title: 标准查询运算符概述 (C#)
description: LINQ 标准查询运算符提供包括 C# 中的筛选、投影、聚合和排序在内的查询功能。
ms.date: 07/20/2015
ms.assetid: 812fa119-5f65-4139-b4fa-55dccd8dc3ac
ms.openlocfilehash: 1ff98e47641dbe7a884b7d6c7758c1fe61b95091
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178770"
---
# <a name="standard-query-operators-overview-c"></a>标准查询运算符概述 (C#)

*标准查询运算符*是组成 LINQ 模式的方法。 这些方法中的大多数都作用于序列；其中序列指其类型实现 <xref:System.Collections.Generic.IEnumerable%601> 接口或 <xref:System.Linq.IQueryable%601> 接口的对象。 标准查询运算符提供包括筛选、投影、聚合、排序等在内的查询功能。  
  
 共有两组 LINQ 标准查询运算符，一组作用于类型 <xref:System.Collections.Generic.IEnumerable%601> 的对象，另一组作用于类型 <xref:System.Linq.IQueryable%601> 的对象。 构成每个集合的方法分别是 <xref:System.Linq.Enumerable> 和 <xref:System.Linq.Queryable> 类的静态成员。 这些方法被定义为作为方法运行目标的类型的*扩展方法*。 可以使用静态方法语法或实例方法语法来调用扩展方法。  
  
 此外，多个标准查询运算符方法作用于那些基于 <xref:System.Collections.Generic.IEnumerable%601> 或 <xref:System.Linq.IQueryable%601> 的类型外的类型。 <xref:System.Linq.Enumerable> 类型定义了两种这样的方法，这两种方法都作用于类型 <xref:System.Collections.IEnumerable> 的对象。 这些方法（<xref:System.Linq.Enumerable.Cast%60%601%28System.Collections.IEnumerable%29> 和 <xref:System.Linq.Enumerable.OfType%60%601%28System.Collections.IEnumerable%29>）均允许在 LINQ 模式中查询非参数化或非泛型集合。 这些方法通过创建一个强类型的对象集合来实现这一点。 <xref:System.Linq.Queryable> 类定义了两种类似的方法 <xref:System.Linq.Queryable.Cast%60%601%28System.Linq.IQueryable%29> 和 <xref:System.Linq.Queryable.OfType%60%601%28System.Linq.IQueryable%29>，这两种方法都作用于类型 <xref:System.Linq.Queryable> 的对象。  
  
 各个标准查询运算符在执行时间上有所不同，具体情况取决于它们是返回单一值还是值序列。 返回单一实例值的这些方法（例如 <xref:System.Linq.Enumerable.Average%2A> 和 <xref:System.Linq.Enumerable.Sum%2A>）立即执行。 返回序列的方法会延迟查询执行，并返回一个可枚举的对象。  
  
 对于在内存中集合上运行的方法（即扩展 <xref:System.Collections.Generic.IEnumerable%601> 的那些方法），返回的可枚举对象将捕获传递到方法的参数。 在枚举该对象时，将使用查询运算符的逻辑，并返回查询结果。  
  
 相反，扩展 <xref:System.Linq.IQueryable%601> 的方法不会实现任何查询行为。 它们生成一个表示要执行的查询的表达式树。 源 <xref:System.Linq.IQueryable%601> 对象执行查询处理。  
  
 可以在一个查询中将对查询方法的调用链接在一起，这就使得查询的复杂性可能会变得不确定。  
  
 下面的代码示例演示如何使用标准查询运算符来获取有关序列的信息。  
  
```csharp  
string sentence = "the quick brown fox jumps over the lazy dog";  
// Split the string into individual words to create a collection.  
string[] words = sentence.Split(' ');  
  
// Using query expression syntax.  
var query = from word in words  
            group word.ToUpper() by word.Length into gr  
            orderby gr.Key  
            select new { Length = gr.Key, Words = gr };  
  
// Using method-based query syntax.  
var query2 = words.  
    GroupBy(w => w.Length, w => w.ToUpper()).  
    Select(g => new { Length = g.Key, Words = g }).  
    OrderBy(o => o.Length);  
  
foreach (var obj in query)  
{  
    Console.WriteLine("Words of length {0}:", obj.Length);  
    foreach (string word in obj.Words)  
        Console.WriteLine(word);  
}  
  
// This code example produces the following output:  
//  
// Words of length 3:  
// THE  
// FOX  
// THE  
// DOG  
// Words of length 4:  
// OVER  
// LAZY  
// Words of length 5:  
// QUICK  
// BROWN  
// JUMPS
```  
  
## <a name="query-expression-syntax"></a>查询表达式语法  

 某些使用更频繁的标准查询运算符具有专用的 C# 和 Visual Basic 语言关键字语法，使用这些语法可以在“查询表达式”中调用这些运算符 。 有关具有专用关键字及其对应语法的标准查询运算符的详细信息，请参阅[标准查询运算符的查询表达式语法 (C#)](./query-expression-syntax-for-standard-query-operators.md)。  
  
## <a name="extending-the-standard-query-operators"></a>扩展标准查询运算符  

 通过创建适合于目标域或技术的特定于域的方法，可以增大标准查询运算符的集合。 也可以用自己的实现来替换标准查询运算符，这些实现提供诸如远程计算、查询转换和优化之类的附加服务。 有关示例，请参见 <xref:System.Linq.Enumerable.AsEnumerable%2A>。  
  
## <a name="related-sections"></a>相关章节  

 通过以下链接可转到一些文章，这些文章基于功能提供有关各种标准查询运算符的附加信息。  
  
 [对数据进行排序 (C#)](./sorting-data.md)  
  
 [集运算 (C#)](./set-operations.md)  
  
 [筛选数据 (C#)](./filtering-data.md)  
  
 [限定符运算 (C#)](./quantifier-operations.md)  
  
 [投影运算 (C#)](./projection-operations.md)  
  
 [数据分区 (C#)](./partitioning-data.md)  
  
 [联接运算 (C#)](./join-operations.md)  
  
 [数据分组 (C#)](./grouping-data.md)  
  
 [生成运算 (C#)](./generation-operations.md)  
  
 [相等运算 (C#)](./equality-operations.md)  
  
 [元素运算 (C#)](./element-operations.md)  
  
 [转换数据类型 (C#)](./converting-data-types.md)  
  
 [串联运算 (C#)](./concatenation-operations.md)  
  
 [聚合运算 (C#)](./aggregation-operations.md)  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq.Enumerable>
- <xref:System.Linq.Queryable>
- [LINQ 查询简介 (C#)](./introduction-to-linq-queries.md)
- [标准查询运算符的查询表达式语法 (C#)](./query-expression-syntax-for-standard-query-operators.md)
- [标准查询运算符按执行方式的分类 (C#)](./classification-of-standard-query-operators-by-manner-of-execution.md)
- [扩展方法](../../classes-and-structs/extension-methods.md)
