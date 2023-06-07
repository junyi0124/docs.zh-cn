---
title: 如何：以特定类型返回 LINQ 查询结果
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], specific type returned
- projection [LINQ in Visual Basic]
- anonymous types [Visual Basic]
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
ms.assetid: 621bb10a-e5d7-44fb-a025-317964b19d92
ms.openlocfilehash: 249c3eebaeec3d09a297fead07ab056caff1b618
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071802"
---
# <a name="how-to-return-a-linq-query-result-as-a-specific-type-visual-basic"></a>如何：以特定类型返回 LINQ 查询结果 (Visual Basic)

使用语言集成查询 (LINQ) 可以轻松地访问数据库信息和执行查询。 默认情况下，LINQ 查询将对象列表作为匿名类型返回。 您还可以通过使用子句指定查询返回特定类型的列表 `Select` 。  
  
 下面的示例演示如何创建一个新的应用程序，用于对 SQL Server 数据库执行查询并将结果投影为特定的命名类型。 有关详细信息，请参阅 [匿名类型](../objects-and-classes/anonymous-types.md) 和 [Select 子句](../../../language-reference/queries/select-clause.md)。  
  
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
  
### <a name="to-add-tables-to-query-to-the-or-designer"></a>添加要查询到 O/R 设计器的表  
  
1. 在**服务器资源管理器** / **数据库资源管理器**中，展开与 Northwind 数据库的连接。 展开 **“表”** 文件夹。  
  
     如果关闭了 O/R 设计器，则可以通过双击先前添加的 northwind 文件来重新打开它。  
  
2. 单击 "Customers" 表，并将其拖到设计器的左窗格中。  
  
     设计器将为您的项目创建一个新 `Customer` 的对象。 可以将查询结果投影为 `Customer` 类型或创建的类型。 此示例将在后面的过程中创建新类型，并将查询结果投影为该类型。  
  
3. 保存更改并关闭设计器。  
  
4. 保存你的项目。  
  
### <a name="to-add-code-to-query-the-database-and-display-the-results"></a>添加代码以查询数据库并显示结果  
  
1. 从 " **工具箱**" 中，将 <xref:System.Windows.Forms.DataGridView> 控件拖动到项目的默认 Windows 窗体 "Form1"。  
  
2. 双击 "Form1" 修改 Form1 类。  
  
3. 在 `End Class` Form1 类的语句后面，添加以下代码以创建一个 `CustomerInfo` 类型以保存此示例的查询结果。  
  
     [!code-vb[VbLINQToSQLHowTos#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form8.vb#16)]  
  
4. 在将表添加到 O/R 设计器后，设计器会将 <xref:System.Data.Linq.DataContext> 对象添加到项目。 此对象包含访问这些表所需的代码，以及访问每个表的单个对象和集合的代码。 <xref:System.Data.Linq.DataContext>项目的对象根据 .dbml 文件的名称进行命名。 对于此项目，该 <xref:System.Data.Linq.DataContext> 对象的名称为 `northwindDataContext` 。  
  
     您可以在代码中创建的一个实例 <xref:System.Data.Linq.DataContext> ，并查询由 O/R 设计器指定的表。  
  
     在 `Load` Form1 类的事件中，添加以下代码以查询作为数据上下文的属性公开的表。 `Select`查询的子句将 `CustomerInfo` 为查询结果的每一项创建一个新类型，而不是匿名类型。  
  
     [!code-vb[VbLINQToSQLHowTos#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form8.vb#15)]  
  
5. 按 F5 运行项目并查看结果。  
  
## <a name="see-also"></a>请参阅

- [LINQ](index.md)
- [查询](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext 方法（O/R 设计器）](/visualstudio/data-tools/datacontext-methods-o-r-designer)
