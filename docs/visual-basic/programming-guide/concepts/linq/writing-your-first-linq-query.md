---
title: 编写第一个 LINQ 查询
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], writing
- LINQ queries [Visual Basic]
- LINQ [Visual Basic], writing queries
ms.assetid: 4affb732-3e9b-4479-aa31-1f9bd8183cbe
ms.openlocfilehash: c7d0595b991bdad6ef05b567f95ead8c7fccdbc2
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077275"
---
# <a name="writing-your-first-linq-query-visual-basic"></a>编写第一个 LINQ 查询 (Visual Basic)

*查询*是一种从数据源检索数据的表达式。 查询以专用查询语言表示。 随着时间的推移，为不同类型的数据源开发了不同的语言，例如，用于关系数据库的 SQL 和用于 XML 的 XQuery。 这样，应用程序开发人员就可以为所支持的每种类型的数据源或数据格式学习一种新的查询语言。  
  
 语言集成查询 (LINQ) 通过提供一种跨各种数据源和格式使用数据的一致模型，简化了这种情况。 在 LINQ 查询中，始终会用到对象。 您可以使用相同的基本编码模式来查询和转换 XML 文档、SQL 数据库、ADO.NET 数据集和实体中的数据、.NET Framework 集合以及 LINQ 提供程序可用的任何其他源或格式。 本文档介绍了创建和使用基本 LINQ 查询的三个阶段。  
  
## <a name="three-stages-of-a-query-operation"></a>查询操作的三个阶段  

 LINQ 查询操作包含三个操作：  
  
1. 获取数据源。  
  
2. 创建查询。  
  
3. 执行查询。  
  
 在 LINQ 中，查询的执行与查询的创建不同。 不只是通过创建查询来检索任何数据。 这一点将在本主题后面部分进行更详细的讨论。  
  
 下面的示例演示了查询操作的三个部分。 为了便于演示，该示例使用一个整数数组作为方便的数据源。 不过，相同的概念也适用于其他数据源。  
  
> [!NOTE]
> 在 "编译" 页上的 " [项目设计器" (Visual Basic) ](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)上，确保 " **选项推断** " 设置为 **"开"**。  
  
 [!code-vb[VbLINQFirstQuery#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#1)]  
  
 输出：  
  
 `0 2 4 6`  
  
## <a name="the-data-source"></a>数据源  

 由于上一个示例中的数据源是一个数组，因此它隐式支持泛型 <xref:System.Collections.Generic.IEnumerable%601> 接口。 这种情况下，您可以使用数组作为 LINQ 查询的数据源。 支持 <xref:System.Collections.Generic.IEnumerable%601> 或派生接口（如泛型 <xref:System.Linq.IQueryable%601>）的类型称为可查询类型  。  
  
 作为隐式查询类型，数组不需要修改或特殊处理就可以用作 LINQ 数据源。 对于支持的任何集合类型也是如此 <xref:System.Collections.Generic.IEnumerable%601> ，包括泛型 <xref:System.Collections.Generic.List%601> 、 <xref:System.Collections.Generic.Dictionary%602> 和 .NET Framework 类库中的其他类。  
  
 如果尚未实现源数据 <xref:System.Collections.Generic.IEnumerable%601> ，则需要 LINQ 提供程序来实现该数据源的 *标准查询运算符* 的功能。 例如， [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 处理将 XML 文档加载到可查询类型的工作 <xref:System.Xml.Linq.XElement> ，如下面的示例中所示。 有关标准查询运算符的详细信息，请参阅 [标准查询运算符概述 (Visual Basic) ](standard-query-operators-overview.md)。  
  
 [!code-vb[VbLINQFirstQuery#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#2)]  
  
 使用 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] ，你可以在设计时先使用 Visual studio 中的 [visual studio 中的 LINQ to SQL 工具](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2) 来创建对象关系映射。 针对这些对象编写查询，然后由 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] 在运行时处理与数据库的通信。 在下面的示例中， `customers` 表示数据库中的特定表，并 <xref:System.Data.Linq.Table%601> 支持泛型 <xref:System.Linq.IQueryable%601> 。  
  
```vb  
' Create a data source from a SQL table.  
Dim db As New DataContext("C:\Northwind\Northwnd.mdf")  
Dim customers As Table(Of Customer) = db.GetTable(Of Customer)  
```  
  
 有关如何创建特定类型的数据源的详细信息，请参阅各种 LINQ 提供程序的文档。  (获取这些提供程序的列表，请参阅 [LINQ (语言集成查询) ](index.md)。 ) 基本规则非常简单： LINQ 数据源是支持泛型接口的任何对象 <xref:System.Collections.Generic.IEnumerable%601> ，或者是从该接口继承的接口。  
  
> [!NOTE]
> <xref:System.Collections.ArrayList>支持非泛型接口的类型（如） <xref:System.Collections.IEnumerable> 也可用作 LINQ 数据源。 有关使用的示例 <xref:System.Collections.ArrayList> ，请参阅 [如何：使用 LINQ (查询 ArrayList Visual Basic) ](how-to-query-an-arraylist-with-linq.md)。  
  
## <a name="the-query"></a>查询  

 在查询中，可以指定要从数据源中检索的信息。 您还可以选择在返回信息之前如何对其进行排序、分组或结构化。 若要启用查询创建，Visual Basic 已将新的查询语法合并到语言中。  
  
 执行时，以下示例中的查询将从整数数组中返回所有偶数 `numbers` 。  
  
 [!code-vb[VbLINQFirstQuery#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#1)]  
  
 查询表达式包含三个子句： `From` 、 `Where` 和 `Select` 。 [基本查询操作 (Visual Basic) ](basic-query-operations.md)中讨论了每个查询表达式子句的特定函数和目的。 有关详细信息，请参阅 [查询](../../../language-reference/queries/index.md)。 请注意，在 LINQ 中，查询定义通常存储在变量中，并在以后执行。 查询变量（如 `evensQuery` 前面的示例中的）必须是可查询的类型。 的类型 `evensQuery` 为 `IEnumerable(Of Integer)` ，由编译器使用局部类型推理分配。  
  
 请记住，查询变量本身不会执行任何操作，也不会返回任何数据。 它仅存储查询定义。 在上面的示例中，它是 `For Each` 执行查询的循环。  
  
## <a name="query-execution"></a>查询执行  

 查询执行与查询创建分离。 查询创建定义查询，但执行由不同的机制触发。 可以在定义查询 *后立即执行查询 () * ，或者可以存储定义，稍后 (*延迟执行*) 执行查询。  
  
### <a name="deferred-execution"></a>延迟执行  

 典型 LINQ 查询类似于上一示例中定义的查询 `evensQuery` 。 它将创建查询，但不会立即执行。 相反，查询定义存储在查询变量中 `evensQuery` 。 稍后，您将执行查询（通常通过使用 `For Each` 返回值序列的循环）或应用标准查询运算符（如 `Count` 或） `Max` 。 此过程称为 " *延迟执行*"。  
  
 [!code-vb[VbLINQFirstQuery#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#7)]  
  
 对于值序列，可以通过在 `For Each` 前面的示例中使用循环 (来访问检索到的数据 `number`) 。 由于查询变量 `evensQuery` 包含查询定义，而不是查询结果，因此可以多次使用查询变量，按所需频率执行查询。 例如，你的应用程序中可能存在由单独的应用程序持续更新的数据库。 创建了从该数据库中检索数据的查询后，可以使用 `For Each` 循环重复执行该查询，每次检索最新的数据。  
  
 下面的示例演示延迟执行的工作方式。 `evensQuery2`使用循环定义并执行后 `For Each` ，如前面的示例中所示，数据源中的某些元素 `numbers` 将发生更改。 然后 `For Each` 再次运行另一个循环 `evensQuery2` 。 由于 `For Each` 循环再次执行查询（使用中的新值），因此结果是不同的 `numbers` 。  
  
 [!code-vb[VbLINQFirstQuery#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#3)]  
  
 输出：  
  
 `Evens in original array:`  
  
 `0  2  4  6`  
  
 `Evens in changed array:`  
  
 `0  10  2  22  8`  
  
### <a name="immediate-execution"></a>立即执行  

 在延迟的查询执行中，查询定义存储在查询变量中以供以后执行。 在 "立即执行" 中，将在定义时执行查询。 当应用需要访问查询结果的单个元素的方法时，将触发执行。 通常，使用返回单个值的标准查询运算符之一来强制立即执行。 例如， `Count` 、 `Max` `Average` 和 `First` 。 这些标准查询运算符会在应用查询后立即执行查询，以便计算并返回单一实例结果。 有关返回单个值的标准查询运算符的详细信息，请参阅 [聚合运算](aggregation-operations.md)、 [元素操作](element-operations.md)和 [限定符运算](quantifier-operations.md)。  
  
 下面的查询返回整数数组中偶数的计数。 查询定义不会保存， `numEvens` 这是一个简单的 `Integer` 。  
  
 [!code-vb[VbLINQFirstQuery#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#4)]  
  
 您可以通过使用方法获得相同的结果 `Aggregate` 。  
  
 [!code-vb[VbLINQFirstQuery#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#5)]  
  
 还可以通过 `ToList` `ToArray` 对查询调用或方法 (立即) 或查询变量 (延迟) ，来强制执行查询，如下面的代码所示。  
  
 [!code-vb[VbLINQFirstQuery#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#6)]  
  
 在前面的示例中， `evensQuery3` 是一个查询变量，但 `evensList` 是一个列表， `evensArray` 是一个数组。  
  
 `ToList` `ToArray` 如果希望立即执行查询并将结果缓存在单个集合对象中，则使用或强制立即执行将特别有用。 有关这些方法的详细信息，请参阅 [转换数据类型](converting-data-types.md)。  
  
 您还可以使用 `IEnumerable` 方法（如方法）来执行查询 <xref:Microsoft.VisualBasic.Collection.System%23Collections%23IEnumerable%23GetEnumerator%2A> 。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 中的 LINQ 入门](getting-started-with-linq.md)
- [局部类型推理](../../language-features/variables/local-type-inference.md)
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [Visual Basic 中的 LINQ 简介](../../language-features/linq/introduction-to-linq.md)
- [LINQ](../../language-features/linq/index.md)
- [查询](../../../language-reference/queries/index.md)
