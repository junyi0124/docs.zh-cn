---
title: LINQ 介绍
ms.date: 08/28/2018
helpviewer_keywords:
- queries [LINQ in Visual Basic], about LINQ in Visual Basic queries
- query execution [LINQ in Visual Basic]
- range variables [Visual Basic]
- LINQ [Visual Basic]
- LINQ [Visual Basic], about LINQ in Visual Basic
- query expressions [LINQ in Visual Basic]
- LINQ
- deferred execution
- iteration variables [Visual Basic]
ms.assetid: 3047d86e-0d49-40e2-928b-dc02e46c7984
ms.openlocfilehash: 00022fc7790548dbc0ed8018f202e136bdbcc033
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075247"
---
# <a name="introduction-to-linq-in-visual-basic"></a>Visual Basic 中的 LINQ 简介

语言集成查询 (LINQ) 将查询功能添加到 Visual Basic，并在使用各种数据时提供简单而强大的功能。 LINQ 会引入查询作为 Visual Basic 语言的一部分，而不是将查询发送到要处理的数据库，或对要搜索的每个数据类型使用不同的查询语法。 它使用统一语法，而不考虑数据的类型。  
  
 LINQ 使你能够从 SQL Server 数据库、XML、内存中数组和集合、ADO.NET 数据集或支持 LINQ 的任何其他远程或本地数据源查询数据。 您可以通过常用 Visual Basic 语言元素来执行所有这些操作。 因为查询是用 Visual Basic 语言编写的，所以查询结果以强类型对象的形式返回。 这些对象支持 IntelliSense，这使你能够更快地编写代码，并在编译时（而不是在运行时）捕获查询中的错误。 可将 LINQ 查询用作其他查询的源，以便缩小结果的范围。 还可将它们绑定到控件，使用户能够轻松地查看和修改查询结果。  
  
 例如，下面的代码示例显示一个 LINQ 查询，它从集合返回一个客户列表并根据位置对其进行分组。  
  
 [!code-vb[VbVbalrIntroToLINQ#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#1)]  
  
## <a name="running-the-examples"></a>运行示例  

 若要运行简介和 [LINQ 查询](#structure-of-a-linq-query) 部分的结构中的示例，请包含以下代码，该代码将返回客户和订单的列表。  
  
 [!code-vb[VbVbalrIntroToLINQ#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#31)]  
  
## <a name="linq-providers"></a>LINQ 提供程序  

 *Linq 提供程序*将 Visual Basic LINQ 查询映射到要查询的数据源。 编写 LINQ 查询时，该提供程序将接受该查询，并将其转换为数据源能够执行的命令。 该提供程序还将来自源的数据转换为构成查询结果的对象。 最后，当你将更新发送至数据源时，它又将这些对象转换为数据。  
  
 Visual Basic 包括以下 LINQ 提供程序。  
  
|提供程序|描述|  
|---|---|  
|LINQ to Objects|通过 LINQ to Objects 提供程序，可以查询内存中集合和数组。 如果对象支持 <xref:System.Collections.IEnumerable> 或 <xref:System.Collections.Generic.IEnumerable%601> 接口，则可以通过 LINQ to Objects 提供程序对其进行查询。<br /><br /> 可以通过导入命名空间来启用 LINQ to Objects 提供程序 <xref:System.Linq> ，默认情况下，将为所有 Visual Basic 项目导入该命名空间。<br /><br /> 有关 LINQ to Objects 提供程序的详细信息，请参阅 [LINQ to Objects](../../concepts/linq/linq-to-objects.md)。|  
|LINQ to SQL|通过 LINQ to SQL 提供程序，可以查询和修改 SQL Server 数据库中的数据。 这样就可以轻松将应用程序的对象模型映射到数据库中的表和对象。<br /><br /> Visual Basic 通过包含对象关系设计器 (O/R 设计器) 来更轻松地使用 LINQ to SQL。 此设计器用于在应用程序中创建映射到数据库中的对象的对象模型。 O/R 设计器还提供了将存储过程和函数映射到对象的功能 <xref:System.Data.Linq.DataContext> ，该对象管理与数据库的通信并存储开放式并发检查的状态。<br /><br /> 有关 LINQ to SQL 提供程序的详细信息，请参阅 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)。 有关对象关系设计器的详细信息，请参阅 [Visual Studio 中的 LINQ to SQL 工具](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)。|  
|LINQ to XML|通过 LINQ to XML 提供程序，可以查询和修改 XML。 可以修改内存中 XML，也可以从文件加载 XML 以及将 XML 保存到文件。<br /><br /> 此外，LINQ to XML 提供程序启用 XML 文本和 XML 轴属性，使你能够直接在 Visual Basic 代码中编写 XML。 有关详细信息，请参阅 [XML](../xml/index.md)。|  
|LINQ to DataSet|使用 LINQ to DataSet 提供程序可以查询和更新 ADO.NET 数据集中的数据。 可以将 LINQ 的强大功能添加到使用数据集的应用程序，以便简化和扩展查询、聚合和更新数据集中数据的功能。<br /><br /> 有关详细信息，请参阅 [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)。|  
  
## <a name="structure-of-a-linq-query"></a>LINQ 查询的结构  

 LINQ 查询（通常称为 *查询表达式*）由用于标识查询的数据源和迭代变量的查询子句组合而成。 查询表达式还可以包含排序、筛选、分组和联接的说明或要对源数据应用的计算。 查询表达式语法类似于 SQL的语法；因此，你可能发现该语法大都非常熟悉。  
  
 查询表达式以 `From` 子句开头。 此子句标识查询的源数据和用于分别表示源数据中每个元素的变量。 这些变量称为 " *范围变量* " 或 " *迭代变量*"。 `From` 子句是查询所必需的，但 `Aggregate` 查询除外，在该查询中 `From` 子句是可选的。 在 `From` 或 `Aggregate` 子句中标识查询的范围和源后，即可包括查询子句的任意组合来优化查询。 有关查询子句的详细信息，请参阅本主题后面的 Visual Basic LINQ 查询运算符。 例如，下面的查询将客户数据源集合标识为 `customers` 变量和一个名为 `cust` 的迭代变量。  
  
 [!code-vb[VbVbalrIntroToLINQ#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#2)]  
  
 此示例本身是一个有效查询；但是，如果添加更多查询子句以优化结果，查询将变得更加强大。 例如，可以添加 `Where` 子句，以便按一个或多个值筛选结果。 查询表达式都是单行代码；只能将其他查询子句附加到查询的末尾。 您可以通过使用下划线 () 行继续符，来分解多行文本中的查询，从而提高可读性 \_ 。 下面的代码示例演示了包含 `Where` 子句的查询示例。  
  
 [!code-vb[VbVbalrIntroToLINQ#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#3)]  
  
 另一个功能强大的查询子句是 `Select` 子句，它能够从数据源仅返回所选字段。 LINQ 查询返回强类型化对象的可枚举集合。 查询可以返回匿名类型或具名类型的集合。 可以使用 `Select` 子句从数据源仅返回单个字段。 执行此操作时，返回的集合类型为该单个字段的类型。 也可以使用 `Select` 子句从数据源返回多个字段。 执行此操作时，返回的集合类型为新的匿名类型。 还可以将由查询返回的字段和指定的具名类型字段进行匹配。 下面的代码示例演示了一个查询表达式，它返回一个匿名类型的集合，该集合具有使用来自数据源中所选字段的数据填充的成员。  
  
 [!code-vb[VbVbalrIntroToLINQ#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#4)]  
  
 还可使用 LINQ 查询合并多个数据源组并返回单个结果。 可以使用一个或多个 `From` 子句，或者使用 `Join` 或 `Group Join` 查询子句来完成此操作。 下面的代码示例演示了一个查询表达式，它合并客户和订单数据，并返回包含客户和订单数据的匿名类型的集合。  
  
 [!code-vb[VbVbalrIntroToLINQ#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#5)]  
  
 可以使用 `Group Join` 子句创建包含客户对象集合的分层查询结果。 每个客户对象都具有一个包含该客户所有订单的集合的属性。 下面的代码示例演示了一个查询表达式，它将客户和订单数据合并为分层结果，并返回一个匿名类型的集合。 查询返回一个包括 `CustomerOrders` 属性的类型，该属性包含客户订单数据的集合。 它还包括 `OrderTotal` 属性，该属性包含客户所有订单的总计值之和。 （此查询等效于 LEFT OUTER JOIN。）  
  
 [!code-vb[VbVbalrIntroToLINQ#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#6)]  
  
 还有几个其他 LINQ 查询运算符，可用它们创建功能强大的查询表达式。 本主题的下一部分将讨论查询表达式中可以包括的各种查询子句。 有关 Visual Basic 查询子句的详细信息，请参阅 [查询](../../../language-reference/queries/index.md)。  
  
## <a name="visual-basic-linq-query-operators"></a>Visual Basic LINQ 查询运算符  

你可以调用 <xref:System.Linq> 命名空间和支持 LINQ 查询的其他命名空间中的类包括的方法，以便根据应用程序的需要创建和完善查询。 Visual Basic 包括以下常见查询子句的关键字。 有关 Visual Basic 查询子句的详细信息，请参阅 [查询](../../../language-reference/queries/index.md)。

### <a name="from-clause"></a>From 子句

要开始查询，需要使用[ `From` 子句](../../../language-reference/queries/from-clause.md)或 `Aggregate` 子句。 `From` 子句可指定查询的源集合和迭代变量。 例如：

 [!code-vb[VbVbalrIntroToLINQ#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#7)]

### <a name="select-clause"></a>Select 子句

可选。 [ `Select` 子句](../../../language-reference/queries/select-clause.md)为查询声明一组迭代变量。 例如：

 [!code-vb[VbVbalrIntroToLINQ#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#8)]

如果未指定 `Select` 子句，则该查询的迭代变量将由 `From` 或 `Aggregate` 子句指定的迭代变量组成。

### <a name="where-clause"></a>Where 子句

可选。 [ `Where` 子句](../../../language-reference/queries/where-clause.md)指定查询的筛选条件。 例如：

 [!code-vb[VbVbalrIntroToLINQ#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#9)]

### <a name="order-by-clause"></a>Order By 子句

可选。 [ `Order By` 子句](../../../language-reference/queries/order-by-clause.md)指定查询中列的排序顺序。 例如：

 [!code-vb[VbVbalrIntroToLINQ#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#10)]

### <a name="join-clause"></a>Join 子句

可选。 [ `Join` 子句](../../../language-reference/queries/join-clause.md)将两个集合组合为单个集合。 例如：

 [!code-vb[VbVbalrIntroToLINQ#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#11)]

### <a name="group-by-clause"></a>Group By 子句

可选。 [ `Group By` 子句](../../../language-reference/queries/group-by-clause.md)对查询结果的元素进行分组。 它可用于将聚合函数应用于每个组。 例如：

 [!code-vb[VbVbalrIntroToLINQ#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#12)]

### <a name="group-join-clause"></a>Group Join 子句

可选。 [ `Group Join` 子句](../../../language-reference/queries/group-join-clause.md)将两个集合组合为单个分层集合。 例如：

 [!code-vb[VbVbalrIntroToLINQ#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#13)]

### <a name="aggregate-clause"></a>Aggregate 子句

要开始查询，需要使用[ `Aggregate` 子句](../../../language-reference/queries/aggregate-clause.md)或 `From` 子句。 `Aggregate` 子句向集合应用一个或多个聚合函数。 例如，可以使用 `Aggregate` 子句来计算查询返回的所有元素的总和，如以下示例中所示。

 [!code-vb[VbVbalrIntroToLINQ#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#14)]

还可以使用 `Aggregate` 子句来修改查询。 例如，可以使用 `Aggregate` 子句，对相关查询集合执行计算。 例如：

 [!code-vb[VbVbalrIntroToLINQ#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#15)]

### <a name="let-clause"></a>Let 子句

可选。 [ `Let` 子句](../../../language-reference/queries/let-clause.md)计算值并将其分配给查询中的新变量。 例如：

 [!code-vb[VbVbalrIntroToLINQ#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#16)]

### <a name="distinct-clause"></a>Distinct 子句

可选。 `Distinct`子句限制当前迭代变量的值，以在查询结果中消除重复值。 例如：

 [!code-vb[VbVbalrIntroToLINQ#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#17)]

### <a name="skip-clause"></a>Skip 子句

可选。 [ `Skip` 子句](../../../language-reference/queries/skip-clause.md)会跳过集合中指定数量的元素，然后返回剩余的元素。 例如：

 [!code-vb[VbVbalrIntroToLINQ#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#18)]

### <a name="skip-while-clause"></a>Skip While 子句

可选。 只要指定的条件为， [ `Skip While` 子句](../../../language-reference/queries/skip-while-clause.md)就会跳过集合中的元素 `true` ，然后返回剩余的元素。 例如：

 [!code-vb[VbVbalrIntroToLINQ#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#19)]

### <a name="take-clause"></a>Take 子句

可选。 [ `Take` 子句](../../../language-reference/queries/take-clause.md)从集合的开头返回指定数量的连续元素。 例如：

 [!code-vb[VbVbalrIntroToLINQ#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#20)]

### <a name="take-while-clause"></a>Take While 子句

可选。 只要指定的条件为， [ `Take While` 子句](../../../language-reference/queries/take-while-clause.md)就包含集合中的元素 `true` ，并跳过剩余的元素。 例如：

 [!code-vb[VbVbalrIntroToLINQ#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#21)]
  
## <a name="use-additional-linq-query-features"></a>使用其他 LINQ 查询功能  
  
可以通过调用 LINQ 提供的可枚举类型和可查询类型的成员来使用其他 LINQ 查询功能。 可以通过对查询表达式的结果调用特定查询运算符来使用这些附加功能。 例如，下面的示例使用方法将 <xref:System.Linq.Enumerable.Union%2A?displayProperty=nameWithType> 两个查询的结果合并为一个查询结果。 它使用 <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> 方法，将查询结果返回为一个泛型列表。
  
 [!code-vb[VbVbalrIntroToLINQ#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#22)]  
  
 有关其他 LINQ 功能的详细信息，请参阅 [标准查询运算符概述](../../concepts/linq/standard-query-operators-overview.md)。  
  
## <a name="connect-to-a-database-by-using-linq-to-sql"></a>使用 LINQ to SQL 连接到数据库  

 在 Visual Basic 中，你将通过使用 LINQ to SQL 文件来标识要访问的 SQL Server 数据库对象，例如表、视图和存储过程。 LINQ to SQL 文件具有 .dbml 扩展名。  
  
 如果与 SQL Server 数据库之间存在有效连接，则可以将 **LINQ to SQL 类** 项模板添加到项目。 此操作将显示对象关系设计器（O/R 设计器）。 O/R 设计器使你能够将你想要在代码中访问的项从**服务器资源管理器** / **数据库资源管理器**拖到设计器图面上。 LINQ to SQL 文件可向项目添加 <xref:System.Data.Linq.DataContext> 对象。 此对象包括你希望访问的表和视图的属性和集合，以及希望调用的存储过程的方法。 保存对 LINQ to SQL (.dbml) 文件所作的更改后，即可通过引用由 O/R 设计器定义的 <xref:System.Data.Linq.DataContext> 对象，在代码中访问这些对象。 项目的 <xref:System.Data.Linq.DataContext> 对象根据 LINQ to SQL 文件的名称进行命名。 例如，名为 Northwind.dbml 的 LINQ to SQL 文件将创建名为 `NorthwindDataContext` 的 <xref:System.Data.Linq.DataContext> 对象。  
  
 有关分步说明的示例，请参阅 [如何：查询数据库](how-to-query-a-database-by-using-linq.md) 和 [如何：调用存储过程](how-to-call-a-stored-procedure-by-using-linq.md)。  
  
## <a name="visual-basic-features-that-support-linq"></a>支持 LINQ 的 Visual Basic 功能  

 Visual Basic 包含了其他一些值得注意的功能，这些功能使得 LINQ 简单，并减少了执行 LINQ 查询所必须编写的代码量。 这些功能包括以下这些：  
  
- **匿名类型**，使您能够根据查询结果创建新类型。  
  
- **隐式类型化的变量**，使您能够延迟指定类型，并让编译器根据查询结果推断类型。  
  
- **扩展方法**，使你可以使用自己的方法扩展现有类型，而无需修改类型本身。  
  
 有关详细信息，请参阅 [支持 LINQ 的 Visual Basic 功能](../../concepts/linq/features-that-support-linq.md)。  
  
## <a name="deferred-and-immediate-query-execution"></a>延迟和立即执行查询

 执行查询与创建查询相互独立。 创建查询后，通过单独的机制触发其执行。 可以在定义查询 *后立即执行查询 () * ，或者可以存储定义，稍后 (*延迟执行*) 执行查询。  
  
 默认情况下，创建查询后，它本身不会立即执行。 而会将查询定义存储在用于引用查询结果的变量中。 当稍后在代码中（如在 `For…Next` 循环中）访问查询结果变量时，将执行该查询。 此过程称为 " *延迟执行*"。  
  
 查询也可以在定义后执行，这称为 " *立即执行*"。 可以通过应用需要访问查询结果中单个元素的方法来触发立即执行。 包括聚合函数（如 `Count`、`Sum`、`Average`、`Min` 或 `Max`）可产生此结果。 有关聚合函数的详细信息，请参阅 [Aggregate 子句](../../../language-reference/queries/aggregate-clause.md)。  
  
 使用 `ToList` 或 `ToArray` 方法也会强制立即执行。 当你希望立即执行查询并缓存查询结果时，这将很有用。 有关这些方法的详细信息，请参阅 [转换数据类型](../../concepts/linq/converting-data-types.md)。  
  
 有关查询执行的详细信息，请参阅 [编写第一个 LINQ 查询](../../concepts/linq/writing-your-first-linq-query.md)。  
  
## <a name="xml-in-visual-basic"></a>Visual Basic 中的 XML  

 Visual Basic 中的 XML 功能包括 XML 文本和 XML 轴属性，这使你可以轻松地在代码中创建、访问、查询和修改 XML。 XML 文本可用于在代码中直接编写 XML。 Visual Basic 编译器将 XML 视为第一类数据对象。  
  
 下面的代码示例演示了如何创建 XML 元素、访问其子元素和属性，以及使用 LINQ 查询该元素的内容。  
  
 [!code-vb[VbXmlSamples#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples3.vb#8)]  
  
 有关详细信息，请参阅 [XML](../xml/index.md)。  
  
## <a name="related-resources"></a>相关资源  
  
|主题|说明|  
|---|---|  
|[XML](../xml/index.md)|介绍 Visual Basic 中可查询的 XML 功能，以及使您能够将 XML 作为 Visual Basic 代码中的第一类数据对象包括在内。|  
|[查询](../../../language-reference/queries/index.md)|提供有关 Visual Basic 中可用的查询子句的参考信息。|  
|[LINQ（语言集成查询）](../../concepts/linq/index.md)|包括常规信息、编程指南和 LINQ 示例。|  
|[LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)|包括常规信息、编程指南和 LINQ to SQL 示例。|  
|[LINQ to Objects](../../concepts/linq/linq-to-objects.md)|包括常规信息、编程指南和 LINQ to Objects 示例。|  
|[LINQ to ADO.NET（门户网站页）](../../concepts/linq/linq-to-adonet-portal-page.md)|包含有关 LINQ to ADO.NET 的一般信息、编程指南和示例的链接。|  
|[LINQ to XML](../../../../standard/linq/linq-xml-overview.md)|包括常规信息、编程指南和 LINQ to XML 示例。|  
  
## <a name="how-to-and-walkthrough-topics"></a>How to 和演练主题

 [如何：查询数据库](how-to-query-a-database-by-using-linq.md)  
  
 [如何：调用存储过程](how-to-call-a-stored-procedure-by-using-linq.md)  
  
 [如何：修改数据库中的数据](how-to-modify-data-in-a-database-by-using-linq.md)  
  
 [如何：使用联接合并数据](how-to-combine-data-with-linq-by-using-joins.md)  
  
 [如何：对查询结果进行排序](how-to-sort-query-results-by-using-linq.md)  
  
 [如何：筛选查询结果](how-to-filter-query-results-by-using-linq.md)  
  
 [如何：对数据进行计数、求和与求平均值计算](how-to-count-sum-or-average-data-by-using-linq.md)  
  
 [如何：查找查询结果中的最小值或最大值](how-to-find-the-minimum-or-maximum-value-in-a-query-result.md)  
  
 [如何：分配存储流程来执行更新、插入和删除操作（O/R 设计器）](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)  
  
## <a name="featured-book-chapters"></a>特色书籍章节  

 第[17 章：](/previous-versions/visualstudio/visual-studio-2008/ff652502(v=orm.10))编程中的 LINQ [Visual Basic 2008](/previous-versions/visualstudio/visual-studio-2008/ff652504(v=orm.10))  
  
## <a name="see-also"></a>请参阅

- [LINQ（语言集成查询）](../../concepts/linq/index.md)
- [Visual Basic 中的 LINQ to XML 概述](../xml/overview-of-linq-to-xml.md)
- [LINQ to DataSet 概述](../../../../framework/data/adonet/linq-to-dataset-overview.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Visual Studio 中的 LINQ to SQL 工具](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)
- [DataContext 方法（O/R 设计器）](/visualstudio/data-tools/datacontext-methods-o-r-designer)
