---
title: 标准查询运算符的查询表达式语法
ms.date: 07/20/2015
ms.assetid: eb978d86-d3b5-497b-95ce-a054bea8f510
ms.openlocfilehash: 57a08f6540cbf3e091ee1b2e202e0e181487e3be
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91090243"
---
# <a name="query-expression-syntax-for-standard-query-operators-visual-basic"></a>标准查询运算符的查询表达式语法 (Visual Basic) 

某些使用更频繁的标准查询运算符具有专用的 Visual Basic 语言关键字语法，使它们可以作为 *查询表达式*的一部分进行调用。 查询表达式是比基于方法的等效项更具可读性的另一种查询表示形式。 查询表达式子句在编译时被转换为对查询方法的调用。  
  
## <a name="query-expression-syntax-table"></a>查询表达式语法表  

 下表列出包含等效查询表达式子句的标准查询运算符。  
  
|方法|Visual Basic 查询表达式语法|  
|------------|------------------------------------------|  
|<xref:System.Linq.Enumerable.All%2A>|`Aggregate … In … Into All(…)`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Any%2A>|`Aggregate … In … Into Any()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Average%2A>|`Aggregate … In … Into Average()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Cast%2A>|`From … As …`<br /><br />  (有关详细信息，请参阅 [From 子句](../../../language-reference/queries/from-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Count%2A>|`Aggregate … In … Into Count()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Distinct%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29>|`Distinct`<br /><br />  (有关详细信息，请参阅 [Distinct 子句](../../../language-reference/queries/distinct-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.GroupBy%2A>|`Group … By … Into …`<br /><br />  (有关详细信息，请参阅 [Group By 子句](../../../language-reference/queries/group-by-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.GroupJoin%60%604%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%2CSystem.Func%7B%60%600%2C%60%602%7D%2CSystem.Func%7B%60%601%2C%60%602%7D%2CSystem.Func%7B%60%600%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%2C%60%603%7D%29>|`Group Join … In … On …`<br /><br />  (有关详细信息，请参阅 [Group Join 子句](../../../language-reference/queries/group-join-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Join%60%604%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%2CSystem.Func%7B%60%600%2C%60%602%7D%2CSystem.Func%7B%60%601%2C%60%602%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%603%7D%29>|`From x In …, y In … Where x.a = b.a`<br /><br /> \- 或 -<br /><br /> `Join … [As …]In … On …`<br /><br />  (有关详细信息，请参阅 [Join 子句](../../../language-reference/queries/join-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.LongCount%2A>|`Aggregate … In … Into LongCount()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Max%2A>|`Aggregate … In … Into Max()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Min%2A>|`Aggregate … In … Into Min()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.OrderBy%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29>|`Order By`<br /><br />  (有关详细信息，请参阅 [Order By 子句](../../../language-reference/queries/order-by-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.OrderByDescending%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29>|`Order By … Descending`<br /><br />  (有关详细信息，请参阅 [Order By 子句](../../../language-reference/queries/order-by-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Select%2A>|`Select`<br /><br />  (有关详细信息，请参阅 [Select 子句](../../../language-reference/queries/select-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.SelectMany%2A>|多个 `From` 子句<br /><br />  (有关详细信息，请参阅 [From 子句](../../../language-reference/queries/from-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Skip%2A>|`Skip`<br /><br />  (有关详细信息，请参阅 [Skip 子句](../../../language-reference/queries/skip-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.SkipWhile%2A>|`Skip While`<br /><br />  (有关详细信息，请参阅 [Skip While 子句](../../../language-reference/queries/skip-while-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Sum%2A>|`Aggregate … In … Into Sum()`<br /><br />  (有关详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Take%2A>|`Take`<br /><br />  (有关详细信息，请参阅 [Take 子句](../../../language-reference/queries/take-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.TakeWhile%2A>|`Take While`<br /><br />  (有关详细信息，请参阅 [Take While 子句](../../../language-reference/queries/take-while-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.ThenBy%60%602%28System.Linq.IOrderedEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29>|`Order By …, …`<br /><br />  (有关详细信息，请参阅 [Order By 子句](../../../language-reference/queries/order-by-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.ThenByDescending%60%602%28System.Linq.IOrderedEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29>|`Order By …, … Descending`<br /><br />  (有关详细信息，请参阅 [Order By 子句](../../../language-reference/queries/order-by-clause.md)。 ) |  
|<xref:System.Linq.Enumerable.Where%2A>|`Where`<br /><br />  (有关详细信息，请参阅 [Where 子句](../../../language-reference/queries/where-clause.md)。 ) |  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq.Enumerable>
- <xref:System.Linq.Queryable>
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [标准查询运算符通过执行 (Visual Basic 的方式分类) ](classification-of-standard-query-operators-by-manner-of-execution.md)
