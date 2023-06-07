---
title: 演练：仅使用存储过程 (C#)
ms.date: 03/30/2017
ms.assetid: ecde4bf2-fa4d-4252-b5e4-96a46b9e097d
ms.openlocfilehash: f980402c976db9ee327a7b726e36a0a4d9d6d73f
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70792110"
---
# <a name="walkthrough-using-only-stored-procedures-c"></a>演练：仅使用存储过程 (C#)

本演练提供了通过仅执行存储过程来访问数据的 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 基本端对端方案。 数据库管理员经常使用此方法来限制数据存储的访问方式。

> [!NOTE]
> 您还可以在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 应用程序中使用存储过程来重写默认行为，尤其是 `Create`、`Update` 和 `Delete` 进程的默认行为。 有关详细信息，请参阅[自定义插入、更新和删除操作](customizing-insert-update-and-delete-operations.md)。

出于本演练的目的，您将使用已映射到 Northwind 示例数据库中的存储过程的两个方法：CustOrdersDetail 和 CustOrderHist。 此映射发生在运行 SqlMetal 命令行工具来生成 C# 文件时。 有关更多信息，请参见本演练后面的“先决条件”一节。

本演练并不依赖于对象关系设计器。 使用 Visual Studio 的开发人员还可以使用 O/R 设计器来实现存储过程功能。 请参阅[Visual Studio 中的 LINQ to SQL 工具](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)。

[!INCLUDE[note_settings_general](../../../../../../includes/note-settings-general-md.md)]

本演练是使用 Visual C# 开发设置编写的。

## <a name="prerequisites"></a>系统必备

本演练需要如下内容：

- 本演练使用专用文件夹（“c:\linqtest7”）来保存文件。 请在开始本演练前创建此文件夹。

- Northwind 示例数据库。

     如果您的开发计算机上没有此数据库，您可以从 Microsoft 下载网站下载它。 有关说明，请参阅[下载示例数据库](downloading-sample-databases.md)。 下载此数据库后，请将 northwnd.mdf 文件复制到 c:\linqtest7 文件夹。

- 从 Northwind 数据库生成的 C# 代码文件。

     本演练是通过使用 SqlMetal 工具以及如下命令行编写的：

     **sqlmetal /code:"c:\linqtest7\northwind.cs" /language:csharp "c:\linqtest7\northwnd.mdf" /sprocs /functions /pluralize**

     有关详细信息，请参阅 [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)。

## <a name="overview"></a>概述

本演练由六项主要任务组成：

- 在 Visual Studio 中设置解决方案。[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]

- 将 System.Data.Linq 程序集添加到项目中。

- 向项目添加数据库代码文件。

- 创建与数据库的连接。

- 设置用户界面。

- 运行和测试应用程序。

## <a name="creating-a-linq-to-sql-solution"></a>创建 LINQ to SQL 解决方案

在第一个任务中，您将创建一个 Visual Studio 解决方案，其中包含生成和运行[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]项目所必需的引用。

### <a name="to-create-a-linq-to-sql-solution"></a>创建 LINQ to SQL 解决方案

1. 在 Visual Studio 的 "**文件**" 菜单上，指向 "**新建**"，然后单击 "**项目**"。

2. 在 "**新建项目**" 对话框的 "**项目类型**" 窗格中，单击 "  **C#视觉对象**"。

3. 在 **“模板”** 窗格中，单击 **“Windows 窗体应用程序”** 。

4. 在 "**名称**" 框中，键入**SprocOnlyApp**。

5. 在 "**位置**" 框中，验证要存储项目文件的位置。

6. 单击 **“确定”** 。

     Windows 窗体设计器即会打开。

## <a name="adding-the-linq-to-sql-assembly-reference"></a>添加 LINQ to SQL 程序集引用

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 程序集未包含在标准的 Windows 窗体应用程序模板中。 您将需要按照以下步骤中的说明自行添加此程序集：

### <a name="to-add-systemdatalinqdll"></a>添加 System.Data.Linq.dll

1. 在**解决方案资源管理器**中，右键单击 "**引用**"，然后单击 "**添加引用**"。

2. 在 "**添加引用**" 对话框中，单击 " **.net**"，单击 "system.web" 程序集，然后单击 **"确定"** 。

     此程序集即被添加到项目中。

## <a name="adding-the-northwind-code-file-to-the-project"></a>将 Northwind 代码文件添加到项目

此步骤假定你已使用 SqlMetal 工具从 Northwind 示例数据库生成代码文件。 有关更多信息，请参见本演练前面部分的“先决条件”一节。

### <a name="to-add-the-northwind-code-file-to-the-project"></a>将 northwind 代码文件添加到项目

1. 在 "**项目**" 菜单上，单击 "**添加现有项**"。

2. 在 "**添加现有项**" 对话框中，转到 "c:\linqtest7\northwind.cs"，然后单击 "**添加**"。

     northwind.cs 文件即被添加到项目中。

## <a name="creating-a-database-connection"></a>创建数据库连接

在此步骤中，您要定义与 Northwind 示例数据库的连接。 本演练使用“c:\linqtest7\northwnd.mdf”作为路径。

### <a name="to-create-the-database-connection"></a>创建数据库连接

1. 在**解决方案资源管理器**中，右键单击**Form1.cs**，然后单击 "**查看代码**"。

2. 将下面的代码键入到 `Form1` 类中：

     [!code-csharp[DLinqWalk4CS#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk4CS/cs/Form1.cs#1)]

## <a name="setting-up-the-user-interface"></a>设置用户界面

在此任务中，你要设置一个界面，供用户执行存储过程以访问数据库中的数据之用。 在您按照本演练开发的应用程序中，用户只能使用应用程序中嵌入的存储过程来访问数据库中的数据。

### <a name="to-set-up-the-user-interface"></a>设置用户界面

1. 返回到 Windows 窗体设计器（**cs [Design]** ）。

2. 在 **“视图”** 菜单上单击 **“工具箱”** 。

     工具箱即会打开。

    > [!NOTE]
    > 单击 "自动**隐藏**" 图钉，使工具箱保持打开状态。

3. 将两个按钮、两个文本框和两个标签从工具箱拖到**Form1**上。

     按照附图排列这些控件。 展开 " **Form1** "，使控件更容易。

4. 右键单击 " **label1**"，然后单击 "**属性**"。

5. 将 " **Text** " 属性从 " **label1** " 更改为 "**输入订单 id：** "。

6. 与**label2**相同的方式，将 " **Text** " 属性从 " **Label2** " 更改为 "**输入 CustomerID：** "。

7. 同样，将**button1**的**Text**属性更改为 "**订单详细信息**"。

8. 将**button2**的**Text**属性更改为**Order History**。

     将这些按钮控件加宽，以使所有文本均可见。

### <a name="to-handle-button-clicks"></a>处理按钮单击

1. 双击 " **Form1** " 上的 "**订单详细信息**"，以在代码编辑器中打开 button1 事件处理程序。

2. 将如下代码键入到 `button1` 处理程序中：

     [!code-csharp[DLinqWalk4CS#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk4CS/cs/Form1.cs#2)]

3. 现在，双击 " **Form1** " 上的 " `button2` **button2** " 打开处理程序

4. 将如下代码键入到 `button2` 处理程序中：

     [!code-csharp[DLinqWalk4CS#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk4CS/cs/Form1.cs#3)]

## <a name="testing-the-application"></a>测试应用程序

现在，可以开始测试您的应用程序了。 请注意，您可对数据存储施加的影响仅限于这两个存储过程能够执行的操作。 这些操作是要根据您输入的所有 orderID 返回相应订单包括的产品，或根据您输入的所有 CustomerID 返回相应客户的产品订购历史记录。

### <a name="to-test-the-application"></a>测试应用程序

1. 按 F5 开始调试。

     此时将显示 Form1。

2. 在 "**输入订单 id** " 框`10249`中，键入，然后单击 "**订单详细信息**"。

     随即会显示一个消息框，其中列出了 10249 号订单中所包括的产品。

     单击 **"确定"** 关闭消息框。

3. 在 "**输入 CustomerID** " 框中`ALFKI`键入，然后单击 "**订单历史记录**"。

     随即会显示一个消息框，其中列出了 ALFKI 客户的订单历史记录。

     单击 **"确定"** 关闭消息框。

4. 在 "**输入订单 id** " 框`123`中，键入，然后单击 "**订单详细信息**"。

     随即会显示一个消息框，其中显示“无结果”。

     单击 **"确定"** 关闭消息框。

5. 在 "**调试**" 菜单上单击 "**停止调试**"。

     调试会话即会关闭。

6. 如果已完成试验，则可以单击 "**文件**" 菜单上的 "**关闭项目**"，并在出现提示时保存项目。

## <a name="next-steps"></a>后续步骤

您可以通过做一些更改来增强此项目的功能。 例如，您可以在列表框中列出可用的存储过程，供用户选择要执行哪些过程。 您还可以将报告的输出以流的方式传输到文本文件。

## <a name="see-also"></a>请参阅

- [通过演练学习](learning-by-walkthroughs.md)
- [存储过程](stored-procedures.md)
