---
title: 演练：操作数据 (C#)
ms.date: 03/30/2017
ms.assetid: 24adfbe0-0ad6-449f-997d-8808e0770d2e
ms.openlocfilehash: fefbee533634ee42785c65e0265ce1e0567561b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164059"
---
# <a name="walkthrough-manipulating-data-c"></a>演练：操作数据 (C#)

本演练提供了用于在数据库中添加、修改和删除数据的基本端对端 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 方案。 您将使用 Northwind 示例数据库的一个副本来添加一位客户，更改该客户的姓名，然后删除一个订单。  
  
 [!INCLUDE[note_settings_general](../../../../../../includes/note-settings-general-md.md)]  
  
 本演练是使用 Visual C# 开发设置编写的。  
  
## <a name="prerequisites"></a>先决条件  

 本演练需要如下内容：  
  
- 本演练使用专用文件夹（“c:\linqtest6”）来保存文件。 请在开始本演练前创建此文件夹。  
  
- Northwind 示例数据库。  
  
     如果您的开发计算机上没有此数据库，您可以从 Microsoft 下载网站下载它。 有关说明，请参阅 [下载示例数据库](downloading-sample-databases.md)。 下载此数据库后，请将 northwnd.mdf 文件复制到 c:\linqtest6 文件夹。  
  
- 从 Northwind 数据库生成的 C# 代码文件。  
  
     您可以使用对象关系设计器或 SQLMetal 工具生成此文件。 本演练是通过使用 SQLMetal 工具以及如下命令行编写的：  
  
     **sqlmetal/code： "c:\linqtest6\northwind.cs"/language： csharp "C:\linqtest6\northwnd.mdf"/pluralize**  
  
     有关详细信息，请参阅 [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)。  
  
## <a name="overview"></a>概述  

 本演练由六项主要任务组成：  
  
- [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]在 Visual Studio 中创建解决方案。  
  
- 向项目添加数据库代码文件。  
  
- 创建新的客户对象。  
  
- 修改客户的联系人姓名。  
  
- 删除订单。  
  
- 将这些更改提交至 Northwind 数据库。  
  
## <a name="creating-a-linq-to-sql-solution"></a>创建 LINQ to SQL 解决方案  

 在第一个任务中，您将创建一个 Visual Studio 解决方案，其中包含生成和运行项目所必需的引用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 。  
  
#### <a name="to-create-a-linq-to-sql-solution"></a>创建 LINQ to SQL 解决方案  
  
1. 在 Visual Studio 的 " **文件** " 菜单上，指向 " **新建**"，然后单击 " **项目**"。  
  
2. 在 "**新建项目**" 对话框的 "**项目类型**" 窗格中，单击 " **Visual c #**"。  
  
3. 在“模板”窗格中，单击“控制台应用程序”。  
  
4. 在 " **名称** " 框中，键入 **LinqDataManipulationApp**。  
  
5. 在 " **位置** " 框中，验证要存储项目文件的位置。  
  
6. 单击“确定”。   
  
## <a name="adding-linq-references-and-directives"></a>添加 LINQ 引用和指令  

 本演练用到默认情况下您的项目中可能未安装的程序集。 如果在您的项目中未将 System.Data.Linq 作为引用列出，请按照以下步骤中的说明添加它。  
  
#### <a name="to-add-systemdatalinq"></a>添加 System.Data.Linq  
  
1. 在 **解决方案资源管理器**中，右键单击 " **引用**"，然后单击 " **添加引用**"。  
  
2. 在 " **添加引用** " 对话框中，单击 " **.net**"，单击 "system.web" 程序集，然后单击 **"确定"**。  
  
     此程序集即被添加到项目中。  
  
3. 在 Program.cs 的顶部添加以下指令：  
  
     [!code-csharp[DLinqWalk3CS#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#1)]  
  
## <a name="adding-the-northwind-code-file-to-the-project"></a>将 Northwind 代码文件添加到项目  

 以下这些步骤假定你已使用 SQLMetal 工具从 Northwind 示例数据库生成代码文件。 有关更多信息，请参见本演练前面部分的“先决条件”一节。  
  
#### <a name="to-add-the-northwind-code-file-to-the-project"></a>将 northwind 代码文件添加到项目  
  
1. 在“项目”**** 菜单上，单击“添加现有项”****。  
  
2. 在 " **添加现有项** " 对话框中，导航到 "c:\linqtest6\northwind.cs"，然后单击 " **添加**"。  
  
     northwind.cs 文件即被添加到项目中。  
  
## <a name="setting-up-the-database-connection"></a>设置数据库连接  

 首先，测试与数据库的连接。 特别要注意，数据库 Northwnd 不含 i 字符。 如果在后面的步骤中产生错误，请检查 northwind.cs 文件以确定 Northwind 分部类的拼法。  
  
#### <a name="to-set-up-and-test-the-database-connection"></a>设置并测试数据库连接  
  
1. 将下面的代码键入或粘贴到 Program 类的 `Main` 方法中：  
  
     [!code-csharp[DLinqWalk3CS#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#2)]  
  
2. 按 F5 在此点测试应用程序。  
  
     此时将打开一个 **控制台** 窗口。  
  
     可以通过在**控制台**窗口中按 enter，或在 Visual Studio 的 "**调试**" 菜单上单击 "**停止调试**" 来关闭应用程序。  
  
## <a name="creating-a-new-entity"></a>创建新实体  

 创建新实体很简单。 可以使用 `Customer` 关键字创建对象（如 `new`）。  
  
 在本节及后续各节中，您将只对本地缓存进行更改。 如果您不调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A>，则不会向数据库发送任何更改，一直到本演练结束都是如此。  
  
#### <a name="to-add-a-new-customer-entity-object"></a>添加新的 Customer 实体对象  
  
1. 通过在 `Customer` 方法中的 `Console.ReadLine();` 前添加如下代码，创建一个新的 `Main`：  
  
     [!code-csharp[DLinqWalk3CS#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#3)]  
  
2. 按 F5 调试解决方案。  
  
3. 在 **控制台** 窗口中按 enter 以停止调试并继续演练。  
  
## <a name="updating-an-entity"></a>更新实体  

 在以下步骤中，您将检索一个 `Customer` 对象并修改其属性之一。  
  
#### <a name="to-change-the-name-of-a-customer"></a>更改客户的姓名  
  
- 将下面的代码添加到 `Console.ReadLine();` 上方：  
  
     [!code-csharp[DLinqWalk3CS#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#4)]  
  
## <a name="deleting-an-entity"></a>删除实体  

 使用同一客户对象，您可以删除第一个订单。  
  
 下面的代码演示如何切断行与行之间的关系，以及如何从数据库中删除行。 请将下面的代码添加到 `Console.ReadLine` 前面，以了解如何可以删除对象：  
  
#### <a name="to-delete-a-row"></a>删除行  
  
- 将下面的代码添加到 `Console.ReadLine();` 上方紧靠它的位置：  
  
     [!code-csharp[DLinqWalk3CS#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#5)]  
  
## <a name="submitting-changes-to-the-database"></a>将更改提交到数据库  

 创建、更新和删除对象所需执行的最后一步是将更改实际提交到数据库。 如不执行这一步，您所做的更改将仅限本地，不会出现在查询结果中。  
  
#### <a name="to-submit-changes-to-the-database"></a>将更改提交到数据库  
  
1. 将下面的代码插入到 `Console.ReadLine` 上方紧靠它的位置：  
  
     [!code-csharp[DLinqWalk3CS#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#6)]  
  
2. 将下面的代码插入到 `SubmitChanges` 后面，以显示提交更改前后的效果：  
  
     [!code-csharp[DLinqWalk3CS#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk3CS/cs/Program.cs#7)]  
  
3. 按 F5 调试解决方案。  
  
4. 在 **控制台** 窗口中按 enter，以关闭应用程序。  
  
> [!NOTE]
> 通过提交更改添加了新的客户后，您无法再次按原样执行此解决方案。 若要再次执行此解决方案，请更改要添加的客户姓名和客户 ID。  
  
## <a name="see-also"></a>请参阅

- [通过演练学习](learning-by-walkthroughs.md)
