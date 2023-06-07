---
title: 如何：使用 LINQ 调用存储过程
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], stored procedure calls
- stored procedures sample [Visual Basic]
- stored procedures [LINQ to SQL]
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 6436d384-d1e0-40aa-8afd-451007477260
ms.openlocfilehash: 7e5fecf0c4c0d3a561ec7d0c4ac03c9d9ce7f759
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075130"
---
# <a name="how-to-call-a-stored-procedure-by-using-linq-visual-basic"></a>如何：使用 LINQ 调用存储过程 (Visual Basic)

使用语言集成查询 (LINQ) 可以轻松地访问数据库信息，其中包括数据库对象（如存储过程）。  
  
 下面的示例演示如何创建一个应用程序，用于调用 SQL Server 数据库中的存储过程。 该示例演示如何调用数据库中的两个不同的存储过程。 每个过程都返回查询的结果。 一个过程采用输入参数，而另一个过程不接受参数。  
  
 本主题中的示例使用 Northwind 示例数据库。 如果你的开发计算机上没有此数据库，可以从 Microsoft 下载中心进行下载。 有关说明，请参阅 [下载示例数据库](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md)。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-connection-to-a-database"></a>创建与数据库的连接  
  
1. 在 Visual Studio 中， **Server Explorer** / **Database Explorer**单击**Server Explorer** / "**视图**" 菜单上的 "服务器资源管理器"**数据库资源管理器**打开服务器资源管理器数据库资源管理器。  
  
2. 右键单击**服务器资源管理器**数据库资源管理器中的 "**数据连接**" / **Database Explorer** ，然后单击 "**添加连接**"。  
  
3. 指定与 Northwind 示例数据库的有效连接。  
  
### <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>添加包含 LINQ to SQL 文件的项目  
  
1. 在 Visual Studio 中的“文件”菜单上，指向“新建”，然后单击“项目”。 选择 Visual Basic **Windows 窗体应用程序** 作为项目类型。  
  
2. 在 **“项目”** 菜单上，单击 **“添加新项”**。 选择 " **LINQ to SQL 类** " 项模板。  
  
3. 命名文件 `northwind.dbml`。 单击“添加”  。 将为 northwind 文件打开对象关系设计器 (O/R 设计器) 。  
  
### <a name="to-add-stored-procedures-to-the-or-designer"></a>将存储过程添加到 O/R 设计器  
  
1. 在**服务器资源管理器** / **数据库资源管理器**中，展开与 Northwind 数据库的连接。 展开 **“存储过程”** 文件夹。  
  
     如果关闭了 O/R 设计器，则可以通过双击先前添加的 northwind 文件来重新打开它。  
  
2. 单击 " **按年份的销售额** " 存储过程，并将其拖到设计器的右窗格中。 单击 **十个最昂贵的产品** 存储过程，将其拖到设计器的右窗格中。  
  
3. 保存更改并关闭设计器。  
  
4. 保存你的项目。  
  
### <a name="to-add-code-to-display-the-results-of-the-stored-procedures"></a>添加代码以显示存储过程的结果  
  
1. 从 " **工具箱**" 中，将 <xref:System.Windows.Forms.DataGridView> 控件拖动到项目的默认 Windows 窗体 "Form1"。  
  
2. 双击 "Form1"，将代码添加到其 `Load` 事件。  
  
3. 将存储过程添加到 O/R 设计器后，设计器将 <xref:System.Data.Linq.DataContext> 为您的项目添加一个对象。 此对象包含访问这些过程所需的代码。 <xref:System.Data.Linq.DataContext>项目的对象根据 .dbml 文件的名称进行命名。 对于此项目，该 <xref:System.Data.Linq.DataContext> 对象的名称为 `northwindDataContext` 。  
  
     您可以在代码中创建的一个实例 <xref:System.Data.Linq.DataContext> ，并调用由 O/R 设计器指定的存储过程方法。 若要绑定到 <xref:System.Windows.Forms.DataGridView> 对象，可能需要通过对存储过程的结果调用方法来强制立即执行查询 <xref:System.Linq.Enumerable.ToList%2A> 。  
  
     向事件中添加以下代码 `Load` ，以调用作为数据上下文的方法公开的任何存储过程。  
  
     [!code-vb[VbLINQtoSQLHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form3.vb#1)]  
    [!code-vb[VbLINQtoSQLHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form3.vb#2)]  
  
4. 按 F5 运行项目并查看结果。  
  
## <a name="see-also"></a>请参阅

- [LINQ](index.md)
- [查询](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext 方法（O/R 设计器）](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [如何：分配存储流程来执行更新、插入和删除操作（O/R 设计器）](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
