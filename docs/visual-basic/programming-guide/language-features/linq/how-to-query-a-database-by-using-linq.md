---
title: 如何：使用 LINQ 查询数据库
ms.date: 07/20/2015
helpviewer_keywords:
- query samples [LINQ]
- database querying [LINQ]
- queries [LINQ in Visual Basic], database querying
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
ms.assetid: bcf5e9c3-a236-4399-9a7f-3991eca7cf56
ms.openlocfilehash: 60dbe3b4e164e266202cac4abaea009869d08cc4
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075260"
---
# <a name="how-to-query-a-database-by-using-linq-visual-basic"></a>如何：使用 LINQ 查询数据库 (Visual Basic)

使用语言集成查询 (LINQ) 可以轻松地访问数据库信息和执行查询。  
  
 下面的示例演示如何创建一个新应用程序，用于对 SQL Server 数据库执行查询。  
  
 本主题中的示例使用 Northwind 示例数据库。 如果你的开发计算机上没有此数据库，可以从 Microsoft 下载中心进行下载。 有关说明，请参阅 [下载示例数据库](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md)。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-create-a-connection-to-a-database"></a>创建与数据库的连接  
  
1. 在 Visual Studio 中， **Server Explorer** / **Database Explorer**单击**Server Explorer** / "**视图**" 菜单上的 "服务器资源管理器"**数据库资源管理器**打开服务器资源管理器数据库资源管理器。  
  
2. 右键单击**服务器资源管理器**数据库资源管理器中的 "**数据连接**" / **Database Explorer** ，然后单击 "**添加连接**"。  
  
3. 指定与 Northwind 示例数据库的有效连接。  
  
## <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>添加包含 LINQ to SQL 文件的项目  
  
1. 在 Visual Studio 中的“文件”菜单上，指向“新建”，然后单击“项目”。 选择 Visual Basic **Windows 窗体应用程序** 作为项目类型。  
  
2. 在 **“项目”** 菜单上，单击 **“添加新项”**。 选择 " **LINQ to SQL 类** " 项模板。  
  
3. 命名文件 `northwind.dbml`。 单击“添加”  。 将为 northwind 文件打开对象关系设计器 (O/R 设计器) 。  
  
## <a name="to-add-tables-to-query-to-the-or-designer"></a>添加要查询到 O/R 设计器的表  
  
1. 在**服务器资源管理器** / **数据库资源管理器**中，展开与 Northwind 数据库的连接。 展开 **“表”** 文件夹。  
  
     如果关闭了 O/R 设计器，则可以通过双击先前添加的 northwind 文件来重新打开它。  
  
2. 单击 "Customers" 表，并将其拖到设计器的左窗格中。 单击 "Orders" 表，并将其拖到设计器的左窗格中。  
  
     设计器将 `Customer` `Order` 为您的项目创建新的和对象。 请注意，设计器将自动检测表之间的关系，并创建相关对象的子属性。 例如，IntelliSense 将显示 `Customer` 对象具有 `Orders` 与该客户相关的所有订单的属性。  
  
3. 保存更改并关闭设计器。  
  
4. 保存你的项目。  
  
## <a name="to-add-code-to-query-the-database-and-display-the-results"></a>添加代码以查询数据库并显示结果  
  
1. 从 " **工具箱**" 中，将 <xref:System.Windows.Forms.DataGridView> 控件拖动到项目的默认 Windows 窗体 "Form1"。  
  
2. 双击 "Form1"，将代码添加到 `Load` 窗体的事件中。  
  
3. 在将表添加到 O/R 设计器后，设计器将 <xref:System.Data.Linq.DataContext> 为您的项目添加一个对象。 除每个表的单个对象和集合外，此对象还包含访问这些表所需的代码。 <xref:System.Data.Linq.DataContext>项目的对象根据 .dbml 文件的名称进行命名。 对于此项目，该 <xref:System.Data.Linq.DataContext> 对象的名称为 `northwindDataContext` 。  
  
     您可以在代码中创建的一个实例 <xref:System.Data.Linq.DataContext> ，并查询由 O/R 设计器指定的表。  
  
     将以下代码添加到 `Load` 事件，以查询作为数据上下文的属性公开的表。  
  
     [!code-vb[VbLINQToSQLHowTos#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form2.vb#3)]  
  
4. 按 F5 运行项目并查看结果。  
  
5. 下面是一些可以尝试执行的其他查询：  
  
     [!code-vb[VbLINQToSQLHowTos#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form2.vb#4)]  
    [!code-vb[VbLINQToSQLHowTos#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form2.vb#5)]  
  
## <a name="see-also"></a>请参阅

- [LINQ](index.md)
- [查询](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext 方法（O/R 设计器）](/visualstudio/data-tools/datacontext-methods-o-r-designer)
