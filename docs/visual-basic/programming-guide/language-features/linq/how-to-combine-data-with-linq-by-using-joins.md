---
title: 如何：通过 LINQ 使用联接合并数据
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], joins
- joins [LINQ in Visual Basic]
- Join clause [LINQ in Visual Basic]
- Group Join clause [Visual Basic]
- joining [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 5b00a478-035b-41c6-8918-be1a97728396
ms.openlocfilehash: ebda8d3b7fa2e712c337ed2c1fadc580bed7fe61
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075065"
---
# <a name="how-to-combine-data-with-linq-by-using-joins-visual-basic"></a>如何：通过 LINQ 使用联接合并数据 (Visual Basic)

Visual Basic 提供了 `Join` 和 `Group Join` 查询子句，使你能够基于集合之间的公共值组合多个集合的内容。 这些值称为 *键值* 。 熟悉关系数据库概念的开发人员将该 `Join` 子句识别为内部联接，将 `Group Join` 子句视为有效的左外部联接。  
  
 本主题中的示例演示了使用 `Join` 和查询子句合并数据的几种方法 `Group Join` 。  
  
## <a name="create-a-project-and-add-sample-data"></a>创建项目并添加示例数据  
  
#### <a name="to-create-a-project-that-contains-sample-data-and-types"></a>创建包含示例数据和类型的项目  
  
1. 若要运行本主题中的示例，请打开 Visual Studio 并添加新的 Visual Basic 控制台应用程序项目。 双击 Visual Basic 创建的 Module1 文件。  
  
2. 本主题中的示例使用 `Person` 和 `Pet` 类型以及以下代码示例中的数据。 将此代码复制到 `Module1` Visual Basic 创建的默认模块。  
  
     [!code-vb[VbLINQHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#1)]  
    [!code-vb[VbLINQHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#2)]  
  
## <a name="perform-an-inner-join-by-using-the-join-clause"></a>使用 Join 子句执行内部联接  

 内部联接结合了两个集合中的数据。 包含指定键值匹配的项。 不会排除在其他集合中没有匹配项的任何集合中的任何项。  
  
 在 Visual Basic 中，LINQ 提供了两个用于执行内部联接的选项：隐式联接和显式联接。  
  
 隐式联接指定要在子句中联接的集合 `From` ，并在子句中标识匹配的键字段 `Where` 。 Visual Basic 基于指定的键字段隐式联接两个集合。  
  
 `Join`当您想要了解要在联接中使用哪些键字段时，可以通过使用子句来指定显式联接。 在这种情况下， `Where` 仍可以使用子句来筛选查询结果。  
  
#### <a name="to-perform-an-inner-join-by-using-the-join-clause"></a>使用 Join 子句执行内部联接  
  
1. 将以下代码添加到 `Module1` 项目中的模块，以查看隐式和显式内部联接的示例。  
  
     [!code-vb[VbLINQHowTos#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#4)]  
  
## <a name="perform-a-left-outer-join-by-using-the-group-join-clause"></a>使用 Group Join 子句执行左外部联接  

 左外部联接包含联接的左侧集合中的所有项，并仅包含联接的右侧集合中的匹配值。 不会从查询结果中排除联接的右侧集合中没有匹配项的联接的任何项。  
  
 `Group Join`子句实际上是左外部联接。 通常称为左外部联接和子句返回的内容之间的区别在于， `Group Join` `Group Join` 子句会对左侧集合中每个项的联接的右端集合中的结果进行分组。 在关系数据库中，左外部联接返回未分组的结果，其中，查询结果中的每一项都包含联接中两个集合中的匹配项。 在这种情况下，将为右侧集合中的每个匹配项重复联接左侧集合中的项。 完成下一个过程后，你将看到如下所示的内容。  
  
 您可以 `Group Join` 通过扩展查询以返回每个分组查询结果的项来检索查询结果作为未分组的结果。 若要实现此目的，必须确保对 `DefaultIfEmpty` 分组集合的方法进行查询。 这可确保联接的左侧集合中的项仍包含在查询结果中，即使它们在右侧集合中没有匹配的结果。 您可以向查询中添加代码，以便在联接的右侧集合中没有匹配的值时提供默认结果值。  
  
#### <a name="to-perform-a-left-outer-join-by-using-the-group-join-clause"></a>使用 Group Join 子句执行左外部联接  
  
1. 将以下代码添加到 `Module1` 项目中的模块，以查看分组左外部联接和未分组左外部联接的示例。  
  
     [!code-vb[VbLINQHowTos#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#3)]  
  
## <a name="perform-a-join-by-using-a-composite-key"></a>使用组合键执行联接  

 可以 `And` 在或子句中使用关键字 `Join` `Group Join` 来标识从要联接的集合中匹配值时要使用的多个键字段。 `And`关键字为要联接的项指定所有指定的键字段必须匹配。  
  
#### <a name="to-perform-a-join-by-using-a-composite-key"></a>使用组合键执行联接  
  
1. 将以下代码添加到 `Module1` 项目中的模块，以查看使用组合键的联接的示例。  
  
     [!code-vb[VbLINQHowTos#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#5)]  
  
## <a name="run-the-code"></a>运行代码  
  
#### <a name="to-add-code-to-run-the-examples"></a>添加用于运行示例的代码  
  
1. 将项目中的模块中的替换为 `Sub Main` `Module1` 以下代码，以运行本主题中的示例。  
  
     [!code-vb[VbLINQHowTos#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#6)]  
  
2. 按 F5 运行示例。  
  
## <a name="see-also"></a>请参阅

- [LINQ](index.md)
- [Visual Basic 中的 LINQ 简介](introduction-to-linq.md)
- [Join 子句](../../../language-reference/queries/join-clause.md)
- [Group Join 子句](../../../language-reference/queries/group-join-clause.md)
- [From 子句](../../../language-reference/queries/from-clause.md)
- [Where 子句](../../../language-reference/queries/where-clause.md)
- [查询](../../../language-reference/queries/index.md)
- [使用 LINQ 进行数据转换 (C#)](../../../../csharp/programming-guide/concepts/linq/data-transformations-with-linq.md)
