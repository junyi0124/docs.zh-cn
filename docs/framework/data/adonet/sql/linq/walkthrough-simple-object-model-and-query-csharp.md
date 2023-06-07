---
title: 演练：简单对象模型和查询 (C#)
description: 按照此演练操作，创建一个用于对示例数据库中的表建模的实体类。 然后，创建一个简单的查询来列出某个位置的客户。
ms.date: 03/30/2017
ms.assetid: 419961cc-92d6-45f5-ae8a-d485bdde3a37
ms.openlocfilehash: 4637fabecc1726d8fec12857a667073912cfbed5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286296"
---
# <a name="walkthrough-simple-object-model-and-query-c"></a>演练：简单对象模型和查询 (C#)

本演练提供了复杂性最小的基本端对端 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 方案。 您将创建一个可为示例 Northwind 数据库中的 Customers 表建模的实体类。 然后您将创建一个简单查询，用于列出位于伦敦的客户。

本演练在设计上是面向代码的，以帮助说明 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 概念。 通常，可以使用对象关系设计器来创建对象模型。

[!INCLUDE[note_settings_general](../../../../../../includes/note-settings-general-md.md)]

本演练是使用 Visual C# 开发设置编写的。

## <a name="prerequisites"></a>先决条件

- 本演练使用专用文件夹（“c:\linqtest5”）来保存文件。 请在开始本演练前创建此文件夹。

- 本演练需要 Northwind 示例数据库。 如果您的开发计算机上没有此数据库，您可以从 Microsoft 下载网站下载它。 有关说明，请参阅[下载示例数据库](downloading-sample-databases.md)。 下载此数据库后，请将文件复制到 c:\linqtest5 文件夹。

## <a name="overview"></a>概述

本演练由六项主要任务组成：

- [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]在 Visual Studio 中创建解决方案。

- 将类映射到数据库表。

- 指定类中的属性表示数据库列。

- 指定到 Northwind 数据库的连接。

- 创建针对该数据库运行的简单查询。

- 执行查询并观察结果。

## <a name="creating-a-linq-to-sql-solution"></a>创建 LINQ to SQL 解决方案

在第一个任务中，您将创建一个 Visual Studio 解决方案，其中包含生成和运行项目所必需的引用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 。

### <a name="to-create-a-linq-to-sql-solution"></a>创建 LINQ to SQL 解决方案

1. 在 Visual Studio 的 "**文件**" 菜单上，指向 "**新建**"，然后单击 "**项目**"。

2. 在 "**新建项目**" 对话框的 "**项目类型**" 窗格中，单击 " **Visual c #**"。

3. 在“模板”**** 窗格中，单击“控制台应用程序”****。

4. 在 "**名称**" 框中，键入**LinqConsoleApp**。

5. 在 "**位置**" 框中，验证要存储项目文件的位置。

6. 单击“确定”。

## <a name="adding-linq-references-and-directives"></a>添加 LINQ 引用和指令

本演练用到默认情况下您的项目中可能未安装的程序集。 如果在你的项目中未将 System.web 列为引用（展开**解决方案资源管理器**中的 "**引用**" 节点），请添加它，如以下步骤中所述。

### <a name="to-add-systemdatalinq"></a>添加 System.Data.Linq

1. 在**解决方案资源管理器**中，右键单击 "**引用**"，然后单击 "**添加引用**"。

2. 在 "**添加引用**" 对话框中，单击 " **.net**"，单击 "system.web" 程序集，然后单击 **"确定"**。

     此程序集即被添加到项目中。

3. 在**Program.cs**的顶部添加以下指令：

     [!code-csharp[DLinqWalk1CS#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#1)]

## <a name="mapping-a-class-to-a-database-table"></a>将类映射到数据库表

在此步骤中，您将创建一个类，并将其映射到数据库表。 这类类称为*实体类*。 请注意，只需添加 <xref:System.Data.Linq.Mapping.TableAttribute> 属性即可完成映射。 <xref:System.Data.Linq.Mapping.TableAttribute.Name%2A> 属性指定数据库中的表的名称。

### <a name="to-create-an-entity-class-and-map-it-to-a-database-table"></a>创建一个实体类并将其映射到数据库表

- 将下面的代码键入或粘贴到 Program.cs 中紧靠在 `Program` 类声明上方的位置：

     [!code-csharp[DLinqWalk1CS#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#2)]

## <a name="designating-properties-on-the-class-to-represent-database-columns"></a>指定类中的属性表示数据库列

在此步骤中，你要完成几项任务。

- 您要使用 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute) 指定实体类中的 `CustomerID` 和 `City` 属性 (Property) 表示数据库表中的列。

- 您要指定 `CustomerID` 属性表示数据库中的主键列。

- 您要指定 `_CustomerID` 和 `_City` 字段用作私有存储字段。 然后，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 就可以直接存储和检索值，而不用使用可能包含业务逻辑的公共访问器。

### <a name="to-represent-characteristics-of-two-database-columns"></a>表示两个数据库列的特性

- 将下面的代码键入或粘贴到 Program.cs 中 `Customer` 类的大括号内。

     [!code-csharp[DLinqWalk1CS#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#3)]

## <a name="specifying-the-connection-to-the-northwind-database"></a>指定到 Northwind 数据库的连接

在此步骤中，要使用 <xref:System.Data.Linq.DataContext> 对象在你基于代码的数据结构和数据库本身之间建立连接。 <xref:System.Data.Linq.DataContext> 是您从数据库中检索对象和提交更改的主要通道。

您还需声明 `Table<Customer>`，用作您针对数据库中 Customers 表的查询的逻辑、类型化表。 您将在后续步骤中创建和执行这些查询。

### <a name="to-specify-the-database-connection"></a>指定数据库连接

- 将下面的代码键入或粘贴到 `Main` 方法中。

     请注意，假定 `northwnd.mdf` 文件位于 linqtest5 文件夹中。 有关更多信息，请参见本演练前面部分的“先决条件”一节。

     [!code-csharp[DLinqWalk1CS#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#4)]

## <a name="creating-a-simple-query"></a>创建简单查询

在此步骤中，您将创建一个查询，查找数据库中的 Customers 表内的哪些客户位于伦敦。 此步骤中的查询代码只描述查询。 它不执行查询。 此方法称为 "*延迟执行*"。 有关详细信息，请参阅 [LINQ 查询简介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)。

您还将生成一个日志输出，显示 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 生成的 SQL 命令。 此日志记录功能（使用 <xref:System.Data.Linq.DataContext.Log%2A>）对调试有帮助，并有助于确定发送给数据库的命令是否准确地表示您的查询。

### <a name="to-create-a-simple-query"></a>创建简单查询

- 将下面的代码键入或粘贴到 `Main` 声明后面的 `Table<Customer>` 方法中。

     [!code-csharp[DLinqWalk1ACS#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1ACS/cs/Program.cs#5)]

## <a name="executing-the-query"></a>执行查询

在此步骤中，您将实际执行查询。 您在前面步骤中创建的查询表达式只有在需要结果时才会进行计算。 当您开始 `foreach` 迭代时，将对数据库执行 SQL 命令，并将对象具体化。

### <a name="to-execute-the-query"></a>执行查询

1. 将下面的代码键入或粘贴到 `Main` 方法的末尾（在查询说明之后）。

     [!code-csharp[DLinqWalk1ACS#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1ACS/cs/Program.cs#6)]

2. 按 F5 调试该应用程序。

    > [!NOTE]
    > 如果你的应用程序产生运行时错误，请参阅[通过演练学习](learning-by-walkthroughs.md)的疑难解答部分。

     控制台窗口中的查询结果应显示如下：

     `ID=AROUT, City=London`

     `ID=BSBEV, City=London`

     `ID=CONSH, City=London`

     `ID=EASTC, City=London`

     `ID=NORTS, City=London`

     `ID=SEVES, City=London`

3. 在控制台窗口中按 Enter 以关闭应用程序。

## <a name="next-steps"></a>后续步骤

[演练：跨关系查询（c #）](walkthrough-querying-across-relationships-csharp.md)主题在本演练结束的位置继续。 "跨关系进行查询" 演练演示如何 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 跨表查询，类似于关系数据库中的*联接*。

如果您希望进行“跨关系查询”演练，请务必保存您刚完成演练的解决方案，这是“跨关系查询”演练的前提条件。

## <a name="see-also"></a>另请参阅

- [通过演练学习](learning-by-walkthroughs.md)
