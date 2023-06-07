---
title: 编写查询
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], writing
- LINQ [Visual Basic], walkthroughs
- LINQ [Visual Basic], writing queries
- writing LINQ queries [Visual Basic]
ms.assetid: f0045808-b9fe-4d31-88d1-473d9957211e
ms.openlocfilehash: c2abca183f1241cff314a4367c7bd9f1b9f239ea
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554588"
---
# <a name="walkthrough-writing-queries-in-visual-basic"></a>演练：用 Visual Basic 编写查询

本演练演示如何使用 Visual Basic 语言功能 (LINQ) 查询表达式编写语言集成查询。 本演练演示如何对学生对象列表创建查询，如何运行查询，以及如何修改查询。 查询包含多个功能，包括对象初始值设定项、本地类型推理和匿名类型。

完成本演练后，你将准备好进入你感兴趣的特定 LINQ 提供程序的示例和文档。 LINQ 提供程序包括 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] 、LINQ to DataSet 和 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 。

## <a name="create-a-project"></a>创建项目

### <a name="to-create-a-console-application-project"></a>创建控制台应用程序项目

1. 启动 Visual Studio。

2. 在 **“文件”** 菜单上，指向 **“新建”** ，然后单击 **“项目”** 。

3. 在 " **已安装的模板** " 列表中，单击 **Visual Basic**。

4. 在项目类型列表中，单击 " **控制台应用程序**"。 在 " **名称** " 框中，键入项目的名称，然后单击 **"确定"**。

    创建一个项目。 默认情况下，它包含对 System.Core.dll 的引用。 此外，"引用" 页上的 "[项目设计器" (Visual Basic) ](/visualstudio/ide/reference/references-page-project-designer-visual-basic)的 "已**导入命名空间**" 列表包括 <xref:System.Linq?displayProperty=nameWithType> 命名空间。

5. 在 "编译" 页上的 " [项目设计器" (Visual Basic) ](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)上，确保 " **选项推断** " 设置为 **"开"**。

## <a name="add-an-in-memory-data-source"></a>添加内存中数据源

此演练中的查询的数据源是对象的列表 `Student` 。 每个 `Student` 对象都包含学生正文中的名字、姓氏、课程年份和学术排名。

### <a name="to-add-the-data-source"></a>添加数据源

- 定义 `Student` 类，并创建类的实例的列表。

  > [!IMPORTANT]
  > `Student`[如何：创建项列表](how-to-create-a-list-of-items.md)中提供了定义类并创建在演练示例中使用的列表所需的代码。 可以将其复制到项目中，并将其粘贴到你的项目中。 新代码将替换在创建项目时显示的代码。

### <a name="to-add-a-new-student-to-the-students-list"></a>向学生列表中添加新的学生

- 按照方法中的模式 `getStudents` 将类的另一个实例添加 `Student` 到列表。 添加学生将向你介绍对象初始值设定项。 有关详细信息，请参阅 [对象初始值设定项：命名类型和匿名类型](../../language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)。

## <a name="create-a-query"></a>创建查询

执行时，此部分中添加的查询将生成一个学生列表，其中的学生排名将其放在前十位。 由于查询 `Student` 每次都选择完整的对象，因此查询结果的类型为 `IEnumerable(Of Student)` 。 但是，查询的类型通常不在查询定义中指定。 相反，编译器将使用局部类型推理来确定类型。 有关详细信息，请参阅 [局部类型推理](../../language-features/variables/local-type-inference.md)。 查询的范围变量用作对 `currentStudent` 源中每个实例的引用 `Student` `students` ，提供对中每个对象的属性的访问 `students` 。

### <a name="to-create-a-simple-query"></a>创建简单查询

1. 查找 `Main` 项目的方法中标记为以下内容的位置：

    [!code-vb[VbLINQWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#1)]

    复制以下代码并将其粘贴到中。

    [!code-vb[VbLINQWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#2)]

2. 将鼠标指针停留 `studentQuery` 在代码中，以验证编译器分配的类型是否为 `IEnumerable(Of Student)` 。

## <a name="run-the-query"></a>运行查询

变量 `studentQuery` 包含查询的定义，而不是运行查询的结果。 用于运行查询的一种典型机制是 `For Each` 循环。 返回序列中的每个元素都通过循环迭代变量来访问。 有关查询执行的详细信息，请参阅 [编写第一个 LINQ 查询](writing-your-first-linq-query.md)。

### <a name="to-run-the-query"></a>运行查询

1. `For Each`在项目中的查询下添加以下循环。

    [!code-vb[VbLINQWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#3)]

2. 将鼠标指针停留在循环控制变量上 `studentRecord` ，以查看其数据类型。 的类型 `studentRecord` 被推断为 `Student` ，因为 `studentQuery` 返回实例的集合 `Student` 。

3. 按 CTRL + F5 生成并运行应用程序。 请注意控制台窗口中的结果。

## <a name="modify-the-query"></a>修改查询

如果查询结果按指定顺序进行扫描，则会更容易。 您可以基于任何可用字段对返回的序列进行排序。

### <a name="to-order-the-results"></a>对结果进行排序

1. 在 `Order By` `Where` 语句和查询的语句之间添加以下子句 `Select` 。 `Order By`子句根据每个学生的姓氏从 A 到 Z 的顺序对结果进行排序。

    ```vb
    Order By currentStudent.Last Ascending
    ```

2. 若要按姓氏再按名字排序，请将这两个字段添加到查询中：

    ```vb
    Order By currentStudent.Last Ascending, currentStudent.First Ascending
    ```

    您还可以指定 `Descending` 从 Z 到 A 的顺序。

3. 按 CTRL + F5 生成并运行应用程序。 请注意控制台窗口中的结果。

### <a name="to-introduce-a-local-identifier"></a>引入本地标识符

1. 添加此部分中的代码，以在查询表达式中引入本地标识符。 本地标识符将保存中间结果。 在下面的示例中， `name` 是一个标识符，该标识符保存学生的名字和姓氏的连接。 本地标识符可用于方便起见，也可通过存储表达式的结果来提高性能，此表达式的计算结果将为多次。

    [!code-vb[VbLINQWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#4)]

2. 按 CTRL + F5 生成并运行应用程序。 请注意控制台窗口中的结果。

### <a name="to-project-one-field-in-the-select-clause"></a>在 Select 子句中投影一个字段

1. 添加 `For Each` 此部分中的查询和循环，以创建一个查询，该查询将生成一个其元素不同于源中的元素的序列。 在下面的示例中，源是对象的集合 `Student` ，但只返回每个对象的一个成员：姓氏为 Garcia 的学生名字。 由于 `currentStudent.First` 是一个字符串，返回的序列的数据类型 `studentQuery3` 是 `IEnumerable(Of String)` 一个字符串序列。 如前面的示例所示，的数据类型的分配 `studentQuery3` 留给编译器通过使用局部类型推理来确定。

    [!code-vb[VbLINQWalkthrough#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#5)]

2. 将鼠标指针停留 `studentQuery3` 在代码中，以验证分配的类型是否为 `IEnumerable(Of String)` 。

3. 按 CTRL + F5 生成并运行应用程序。 请注意控制台窗口中的结果。

### <a name="to-create-an-anonymous-type-in-the-select-clause"></a>在 Select 子句中创建匿名类型

1. 添加此部分中的代码，以了解如何在查询中使用匿名类型。 如果要从数据源返回多个字段，而不是 (前面 `currentStudent` 的示例中的记录) 或 (`First` 前面部分) 中的单个字段）返回多个字段，则可以在查询中使用这些字段。 您可以在子句中指定字段，而不是定义一个新的命名类型（其中包含您想要包括在结果中的字段），而 `Select` 编译器会创建一个匿名类型，并将这些字段作为其属性。 有关详细信息，请参阅[匿名类型](../../language-features/objects-and-classes/anonymous-types.md)。

    下面的示例创建一个查询，该查询返回学术排名介于1到10之间的发言的名称和排名，按学术排名排序。 在此示例中， `studentQuery4` 必须推断的类型 `Select` ，因为子句返回匿名类型的实例，而匿名类型没有可使用的名称。

    [!code-vb[VbLINQWalkthrough#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#6)]

2. 按 CTRL + F5 生成并运行应用程序。 请注意控制台窗口中的结果。

## <a name="additional-examples"></a>其他示例

现在，你已了解基础知识，以下是说明 LINQ 查询的灵活性和强大功能的其他示例列表。 每个示例前面都有其作用的简短说明。 将鼠标指针停留在每个查询的查询结果变量上，以查看推断出的类型。 使用 `For Each` 循环来生成结果。

[!code-vb[VbLINQWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#7)]

## <a name="additional-information"></a>其他信息

熟悉使用查询的基本概念后，便可以阅读感兴趣的特定类型 LINQ 提供程序的文档和示例：

- [LINQ to Objects](linq-to-objects.md)

- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)

- [LINQ to XML](../../../../standard/linq/linq-xml-overview.md)

- [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)

## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) (Visual Basic)](index.md)
- [Visual Basic 中的 LINQ 入门](getting-started-with-linq.md)
- [局部类型推理](../../language-features/variables/local-type-inference.md)
- [对象初始值设定项：命名和匿名类型](../../language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [匿名类型](../../language-features/objects-and-classes/anonymous-types.md)
- [Visual Basic 中的 LINQ 简介](../../language-features/linq/introduction-to-linq.md)
- [LINQ](../../language-features/linq/index.md)
- [查询](../../../language-reference/queries/index.md)
