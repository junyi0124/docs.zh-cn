---
title: 支持 LINQ 的功能
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic, LINQ features
- LINQ [Visual Basic], features supporting LINQ
ms.assetid: c821bb50-b6f6-4cf9-8aba-2717e465bd3a
ms.openlocfilehash: bd63cd36c1f85fd89349293a71ecc5b281165380
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91078301"
---
# <a name="visual-basic-features-that-support-linq"></a>支持 LINQ 的 Visual Basic 功能

 (LINQ) 的名称语言集成查询是指 Visual Basic 中的技术，该技术在语言中直接支持查询语法和其他语言构造。 使用 LINQ，无需学习新语言即可针对外部数据源进行查询。 您可以使用 Visual Basic 对关系数据库、XML 存储或对象中的数据进行查询。 这种将查询功能集成到语言中可实现语法错误和类型安全的编译时检查。 此集成还确保你已了解在 Visual Basic 中编写丰富的可变查询所必须了解的大部分内容。  
  
 以下部分介绍支持 LINQ 的语言构造，以使你能够开始阅读介绍性文档、代码示例和示例应用程序。 您还可以单击链接以查找有关语言功能如何集成以实现语言集成查询的更详细说明。 [演练：在 Visual Basic 中编写查询](walkthrough-writing-queries.md)是一种很好的起点。  
  
## <a name="query-expressions"></a>查询表达式  

 Visual Basic 中的查询表达式可以用类似于 SQL 或 XQuery 的声明性语法来表示。 在编译时，查询语法转换为对 LINQ 提供程序的标准查询运算符扩展方法实现的方法调用。 应用程序通过使用语句指定相应的命名空间来控制哪些标准查询运算符在范围内 `Imports` 。 Visual Basic 查询表达式的语法如下所示：  
  
 [!code-vb[VbLINQVbFeatures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#1)]  
  
 有关详细信息，请参阅[Visual Basic 中的 LINQ 简介](../../language-features/linq/introduction-to-linq.md)。  
  
## <a name="implicitly-typed-variables"></a>隐式类型化变量  

 在声明和初始化变量时，可以让编译器推断并分配类型，而不是在声明和初始化变量时显式指定类型。 这称为 *局部类型推理*。  
  
 推理出的类型为强类型化的变量，就像您显式指定其类型的变量一样。 仅当在方法体中定义本地变量时，局部类型推理才有效。 有关详细信息，请参阅 [选项推断语句](../../../language-reference/statements/option-infer-statement.md) 和 [局部类型推理](../../language-features/variables/local-type-inference.md)。  
  
 下面的示例阐释了局部类型推理。 若要使用此示例，必须将设置 `Option Infer` 为 `On` 。  
  
 [!code-vb[VbLINQVbFeatures#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#2)]  
  
 局部类型推理还可以创建匿名类型，本节稍后将对此进行介绍，这对于 LINQ 查询是必需的。  
  
 在下面的 LINQ 示例中，如果为或，则会发生类型推理 `Option Infer` `On` `Off` 。 如果 `Option Infer` 为 `Off` 且为，则会发生编译时错误 `Option Strict` `On` 。  
  
 [!code-vb[VbLINQVbFeatures#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#3)]  
  
## <a name="object-initializers"></a>对象初始值设定项  

 当必须创建匿名类型来保存查询结果时，可以在查询表达式中使用对象初始值设定项。 它们还可用于在查询之外初始化命名类型的对象。 通过使用对象初始值设定项，你可以在单个行中初始化对象，而无需显式调用构造函数。 假设你有一个名为的类，该类 `Customer` 具有公共 `Name` 和 `Phone` 属性以及其他属性，则可以采用这种方式使用对象初始值设定项：  
  
 [!code-vb[VbLINQVbFeatures#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#4)]  
  
 有关详细信息，请参阅 [对象初始值设定项：命名类型和匿名类型](../../language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)。  
  
## <a name="anonymous-types"></a>匿名类型  

 匿名类型提供了一种简便的方法，可将一组属性暂时分组到要包含在查询结果中的元素中。 这使你可以按任意顺序在查询中选择可用字段的任意组合，而无需为元素定义命名的数据类型。  
  
 *匿名类型*由编译器动态构造。 该类型的名称由编译器分配，并且它可能会随每个新的编译而更改。 因此，不能直接使用该名称。 匿名类型的初始化方式如下：  
  
 [!code-vb[VbLINQVbFeatures#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#5)]  
  
 有关详细信息，请参阅[匿名类型](../../language-features/objects-and-classes/anonymous-types.md)。  
  
## <a name="extension-methods"></a>扩展方法  

 使用扩展方法，可以将方法添加到定义之外的数据类型或接口。 利用此功能，您可以将新方法添加到现有类型，而无需实际修改类型。 标准查询运算符本身就是一组扩展方法，这些方法为实现的任何类型提供 LINQ 查询功能 <xref:System.Collections.Generic.IEnumerable%601> 。 其他要 <xref:System.Collections.Generic.IEnumerable%601> 包括、和的扩展 <xref:System.Linq.Enumerable.Count%2A> <xref:System.Linq.Enumerable.Union%2A> <xref:System.Linq.Enumerable.Intersect%2A> 。  
  
 下面的扩展方法向类添加一个 print 方法 <xref:System.String> 。  
  
 [!code-vb[VbLINQVbFeatures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#6)]  
  
 调用方法的方式类似于的普通实例方法 <xref:System.String> ：  
  
 [!code-vb[VbLINQVbFeatures#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#7)]  
  
 有关详细信息，请参阅[扩展方法](../../language-features/procedures/extension-methods.md)。  
  
## <a name="lambda-expressions"></a>Lambda 表达式  

 Lambda 表达式是没有名称的函数，用于计算并返回单个值。 与命名函数不同，可以同时定义和执行 lambda 表达式。 下面的示例显示4。  
  
 [!code-vb[VbLINQVbFeatures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#8)]  
  
 可以将 lambda 表达式定义分配给变量名，然后使用该名称调用函数。 下面的示例还显示4。  
  
 [!code-vb[VbLINQVbFeatures#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#12)]  
  
 在 LINQ 中，lambda 表达式采用许多标准查询运算符。 编译器创建 lambda 表达式来捕获基本查询方法（如、、、等）中定义的计算 `Where` `Select` `Order By` `Take While` 。  
  
 例如，以下代码定义一个查询，该查询从学生列表返回所有高级学生。  
  
 [!code-vb[VbLINQVbFeatures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#9)]  
  
 查询定义编译为类似于以下示例的代码，它使用两个 lambda 表达式来指定和的参数 `Where` `Select` 。  
  
 [!code-vb[VbLINQVbFeatures#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#10)]  
  
 任一版本都可以使用 `For Each` 循环运行：  
  
 [!code-vb[VbLINQVbFeatures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#11)]  
  
 有关详细信息，请参阅 [Lambda 表达式](../../language-features/procedures/lambda-expressions.md)。  
  
## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) (Visual Basic)](index.md)
- [Visual Basic 中的 LINQ 入门](getting-started-with-linq.md)
- [LINQ 和字符串 (Visual Basic)](linq-and-strings.md)
- [Option Infer 语句](../../../language-reference/statements/option-infer-statement.md)
- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
