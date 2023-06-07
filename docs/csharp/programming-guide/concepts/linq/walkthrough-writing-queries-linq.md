---
title: 演练：用 C# 编写查询 (LINQ)
description: 本演练演示如何在 LINQ 查询表达式中使用 C# 语言功能。 本文使用 Visual Studio 作为开发环境。
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], walkthroughs
- LINQ [C#], writing queries
- queries [LINQ in C#], writing
- writing LINQ queries
ms.assetid: 2962a610-419a-4276-9ec8-4b7f2af0c081
ms.openlocfilehash: bdf91f6f52a68309cfcd276b222083c8cb67a0cc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176235"
---
# <a name="walkthrough-writing-queries-in-c-linq"></a>演练：用 C# 编写查询 (LINQ)

此演练演示用于编写 LINQ 查询表达式的 C# 语言功能。  
  
## <a name="create-a-c-project"></a>创建 C# 项目  
  
> [!NOTE]
> 以下说明适用于 Visual Studio。 如果使用其他开发环境，请创建包含对 System.Core.dll 的引用的控制台项目和用于 <xref:System.Linq?displayProperty=nameWithType> 命名空间的 `using` 指令。  
  
#### <a name="to-create-a-project-in-visual-studio"></a>在 Visual Studio 中创建项目  
  
1. 启动 Visual Studio。  
  
2. 在菜单栏上，依次选择“文件” 、“新建” 、“项目” 。  
  
     **“新建项目”** 对话框随即打开。  
  
3. 依次展开“已安装”、“模板”、“Visual C#”，然后选择“控制台应用程序”。  
  
4. 在“名称”文本框中，输入不同的名称或接受默认名称，然后选择“确定”按钮。  
  
     新项目将出现在“解决方案资源管理器”中。  
  
5. 注意，此项目包含对 System.Core.dll 的引用和用于 <xref:System.Linq?displayProperty=nameWithType> 命名空间的 `using` 指令。  
  
## <a name="create-an-in-memory-data-source"></a>创建内存中的数据源  

 用于查询的数据源是 `Student` 对象的简单列表。 每个 `Student` 记录都有名字、姓氏和整数数组（表示该学生在课堂上的测试分数）。 将此代码复制到项目中。 请注意下列特性：  
  
- `Student` 类包含自动实现的属性。  
  
- 列表中的每个学生都可使用对象初始值设定项进行初始化。  
  
- 列表本身可使用集合初始值设定项进行初始化。  
  
 将在不显式调用任何构造函数和使用显式成员访问的情况下初始化并实例化整个数据结构。 有关这些新功能的详细信息，请参阅[自动实现的属性](../../classes-and-structs/auto-implemented-properties.md)与[对象和集合初始值设定项](../../classes-and-structs/object-and-collection-initializers.md)。  
  
#### <a name="to-add-the-data-source"></a>添加数据源  
  
- 向项目中的 `Program` 类添加 `Student` 类和经过初始化的学生列表。  
  
     [!code-csharp[CsLinqGettingStarted#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#11)]  
  
#### <a name="to-add-a-new-student-to-the-students-list"></a>向学生列表添加新学生  
  
1. 向 `Students` 列表添加一个新 `Student`，并按自己的选择使用名称和测试分数。 尝试键入所有新学生信息，以便更好地了解对象初始值设定项的语法。  
  
## <a name="create-the-query"></a>创建查询  
  
#### <a name="to-create-a-simple-query"></a>创建简单查询  
  
- 在应用程序的 `Main` 方法中，创建简单查询，执行该查询时，将生成所有在第一次测试中分数高于 90 的学生的列表。 注意，由于选定全部 `Student` 对象，所以查询的类型为 `IEnumerable<Student>`。 尽管该代码也可以通过使用 [var](../../../language-reference/keywords/var.md) 关键字来使用隐式类型化，但可以使用显式类型化清楚地展示结果。 （有关 `var` 的详细信息，请参阅[隐式类型化局部变量](../../classes-and-structs/implicitly-typed-local-variables.md)。）  
  
     另请注意，查询的范围变量 `student` 用作指向源中每个 `Student` 引用，提供对每个对象的成员访问。  
  
 [!code-csharp[CsLINQGettingStarted#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#12)]  
  
## <a name="execute-the-query"></a>执行查询  
  
#### <a name="to-execute-the-query"></a>执行查询  
  
1. 现在，编写 `foreach` 循环，用于执行查询。 注意以下有关代码的注意事项：  
  
    - 通过 `foreach` 循环中的迭代变量，可访问返回的序列中的每个元素。  
  
    - 此变量的类型是 `Student`，并且可与查询变量 `IEnumerable<Student>` 的类型兼容。  
  
2. 添加此代码后，生成并运行应用程序，以在“控制台”窗口中查看结果。  
  
 [!code-csharp[CsLINQGettingStarted#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#13)]  
  
#### <a name="to-add-another-filter-condition"></a>添加其他筛选条件  
  
1. 在 `where` 子句中，可以组合多个布尔条件，以便进一步细化查询。 以下代码添加了一个条件，以便该查询返回第一个分数高于 90 分，并且最后一个分数低于 80 分的那些学生。 `where` 子句应与以下代码类似。  
  
    ```csharp
    where student.Scores[0] > 90 && student.Scores[3] < 80  
    ```  
  
     有关详细信息，请参阅 [where 子句](../../../language-reference/keywords/where-clause.md)。  
  
## <a name="modify-the-query"></a>修改查询  
  
#### <a name="to-order-the-results"></a>对结果进行排序  
  
1. 如果结果按某种顺序排列，则浏览结果会更容易。 你可以根据源元素中的任何可访问字段对返回的序列进行排序。 例如，以下 `orderby` 子句将结果按照每个学生的姓氏以字母从 A 到 Z 的顺序排列。 将以下 `orderby` 子句添加到查询中，紧跟 `where` 语句之后、`select` 语句之前：  
  
    ```csharp
    orderby student.Last ascending  
    ```  
  
2. 现在，更改 `orderby` 子句，以便将结果根据第一次测试的分数以倒序（从最高分到最低分）的顺序排列。  
  
    ```csharp
    orderby student.Scores[0] descending  
    ```  
  
3. 更改 `WriteLine` 格式字符串，以便查看分数：  
  
    ```csharp
    Console.WriteLine("{0}, {1} {2}", student.Last, student.First, student.Scores[0]);  
    ```  
  
     有关详细信息，请参阅 [orderby 子句](../../../language-reference/keywords/orderby-clause.md)。  
  
#### <a name="to-group-the-results"></a>对结果进行分组  
  
1. 分组是查询表达式中的强大功能。 包含 group 子句的查询将生成一系列组，每个组本身包含一个 `Key` 和一个序列，该序列由该组的所有成员组成。 以下新查询使用学生的姓的第一个字母作为关键字对学生进行分组。  
  
     [!code-csharp[CsLINQGettingStarted#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#14)]  
  
2. 注意，查询类型现已更改。 该查询现在生成一系列将 `char` 类型作为键的组，以及一系列 `Student` 对象。 由于查询的类型已更改，因此以下代码也将更改 `foreach` 执行循环：  
  
     [!code-csharp[CsLINQGettingStarted#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#15)]  
  
3. 在“控制台”窗口中运行应用程序并查看结果。  
  
     有关详细信息，请参阅 [group 子句](../../../language-reference/keywords/group-clause.md)。  
  
#### <a name="to-make-the-variables-implicitly-typed"></a>对变量进行隐式类型化  
  
1. `IGroupings` 的显式编码 `IEnumerables` 将快速变得冗长。 使用 `var` 可以更方便地编写相同的查询和 `foreach` 循环。 `var` 关键字不会更改对象的类型；它仅指示编译器推断类型。 将 `studentQuery` 和迭代变量 `group` 的类型更改为 `var`，然后重新运行查询。 注意，在内部 `foreach` 循环中，该迭代变量仍类型化为 `Student`，并且查询的工作原理和以前一样。 将 `s` 迭代变量更改为 `var`，然后再次运行查询。 将看到完全相同的结果。  
  
     [!code-csharp[CsLINQGettingStarted#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#16)]  
  
     有关 [var](../../../language-reference/keywords/var.md) 的详细信息，请参阅[隐式类型化局部变量](../../classes-and-structs/implicitly-typed-local-variables.md)。  
  
#### <a name="to-order-the-groups-by-their-key-value"></a>按照键值对组进行排序  
  
1. 运行上一查询时，会发现这些组不是按字母顺序排序的。 若要更改此排序，必须在 `group` 子句后提供 `orderby` 子句。 但若要使用 `orderby` 子句，首先需要一个标识符，用作对 `group` 子句创建的组的引用。 可以使用 `into` 关键字提供该标识符，如下所示：  
  
     [!code-csharp[csLINQGettingStarted#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#17)]  
  
     运行此查询时，将看到这些组现在已按字母顺序排序。  
  
#### <a name="to-introduce-an-identifier-by-using-let"></a>使用 let 引入标识符  
  
1. 可以使用 `let` 关键字来引入查询表达式中任何表达式结果的标识符。 此标识符可以提供方便（如下面的示例所示），也可以通过存储表达式的结果来避免多次计算，从而提高性能。  
  
     [!code-csharp[csLINQGettingStarted#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#18)]  
  
     有关详细信息，请参阅 [let 子句](../../../language-reference/keywords/let-clause.md)。  
  
#### <a name="to-use-method-syntax-in-a-query-expression"></a>在查询表达式中使用方法语法  
  
1. 如 [LINQ 中的查询语法和方法语法](./query-syntax-and-method-syntax-in-linq.md) 中所述，某些查询操作只能使用方法语法来表示。 以下代码为源序列中的每个 `Student` 计算总分，然后对该查询的结果调用 `Average()` 方法来计算班级平均分。
  
     [!code-csharp[csLINQGettingStarted#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#19)]  
  
#### <a name="to-transform-or-project-in-the-select-clause"></a>在 select 子句转换或投影  
  
1. 查询生成的序列的元素与源序列中的元素不同，这种情况很常见。 删除或注释掉以前的查询和执行循环，并将其替换为以下代码。 请注意，该查询将返回字符串序列，而不是 `Students`，这种情况将反映在 `foreach` 循环中。  
  
     [!code-csharp[csLINQGettingStarted#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#20)]  
  
2. 本演练中前面的代码表明班级平均分大约为 334 分。 若要生成总分数高于班级平均分的 `Students` 及其 `Student ID` 的序列，可以在 `select` 语句中使用匿名类型：  
  
     [!code-csharp[csLINQGettingStarted#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#21)]  
  
## <a name="next-steps"></a>后续步骤  

 熟悉了在 C# 中使用查询的基本情况后，便可以开始阅读你感兴趣的具体类型的 LINQ 提供程序的文档和示例：  
  
 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)  
  
 [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)  
  
 [LINQ to XML (C#)](../../../../standard/linq/linq-xml-overview.md)  
  
 [LINQ to Objects (C#)](./linq-to-objects.md)  
  
## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) (C#)](./index.md)
- [LINQ 查询表达式](../../../linq/index.md)
