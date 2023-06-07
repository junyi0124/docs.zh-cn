---
title: 获取 ADO.NET 代码示例的示例 SQL Server 数据库
description: 下载 ADO.NET 文档的代码示例中使用的示例 SQL Server 数据库，以及 SQL Server 和管理工具
ms.date: 01/11/2019
ms.assetid: ef9d69a1-9461-43fe-94bb-7c836754bcb5
ms.openlocfilehash: f7c0d1eb0089a6bfabc92e1deecf563c3e59cc6a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156051"
---
# <a name="get-the-sample-databases-for-adonet-code-samples"></a>获取 ADO.NET 代码示例的示例数据库

文档中的一些示例和演练 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] SQL Server 数据库和 SQL Server Express 的示例。 你可以从 Microsoft 免费下载这些产品。

## <a name="get-the-northwind-sample-database-for-sql-server"></a>获取用于 SQL Server 的 Northwind 示例数据库

`instnwnd.sql`从以下 GitHub 存储库下载脚本，为 SQL Server 创建和加载 Northwind 示例数据库：

[用于 Microsoft SQL Server 的 Northwind 和 pubs 示例数据库](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)

在您可以使用 Northwind 数据库之前，您必须运行下载的 `instnwnd.sql` 脚本文件以通过使用 [SQL Server Management Studio](#get_ssms) 或类似的工具在 SQL Server 的实例上重新创建数据库。 按照存储库中的自述文件中的说明进行操作。

> [!TIP]
> 如果正在寻找用于 Microsoft Access 的 Northwind 数据库，请参阅 [安装用于 Microsoft access 的 northwind 示例数据库](#northwind_access)。

## <a name="get-the-northwind-sample-database-for-microsoft-access"></a><a name="northwind_access"></a> 获取 Microsoft Access 的 Northwind 示例数据库

Microsoft 下载中心不提供用于 Microsoft Access 的 Northwind 示例数据库。 若要直接从 Access 中安装 Northwind，请执行以下操作：

1. 打开访问。

1. 在 "**搜索联机模板**" 框中输入**Northwind** ，然后选择**Enter**。

1. 在结果屏幕上，选择 " **Northwind**"。 此时将打开一个新窗口，其中包含 Northwind 数据库的说明。

1. 在新窗口中，在 " **文件名** " 文本框中提供 Northwind 数据库副本的文件名。

1. 选择“创建”。 Access 会下载 Northwind 数据库并准备好文件。

1. 此过程完成后，数据库将打开并显示欢迎屏幕。

## <a name="get-the-adventureworks-sample-database-for-sql-server"></a>获取用于 SQL Server 的 AdventureWorks 示例数据库

从以下 GitHub 存储库下载适用于 SQL Server 的 AdventureWorks 示例数据库：

[AdventureWorks sample databases](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)（AdventureWorks 示例数据库）

将一个数据库备份 (\* .bak) 文件下载完成后，使用 SQL Server Management Studio (SSMS) 将备份还原到 SQL Server 的实例。 请参阅 [获取 SQL Server Management Studio](#get_ssms)。

## <a name="get-sql-server-management-studio"></a><a name="get_ssms"></a> 获取 SQL Server Management Studio

若要查看或修改已下载的数据库，可以使用 SQL Server Management Studio (SSMS) 。 从以下页面下载 SSMS：

[下载 SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms)

你还可以在 Visual Studio 集成开发环境 (IDE) 中查看和管理数据库。 在 [Visual Studio](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019)中，从 **SQL Server 对象资源管理器**连接到数据库，或创建到 **服务器资源管理器**中的数据库的数据连接。 从 " **视图** " 菜单打开这些 "资源管理器" 窗格。

## <a name="get-sql-server-express"></a><a name="get_sql"></a> 获取 SQL Server Express

SQL Server Express 是免费的入门级版 SQL Server，可与应用程序一起分发。 从以下页面下载 SQL Server Express：
  
[SQL Server Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express)

如果你使用的是 [Visual studio](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019)，则 visual Studio 免费社区版以及专业版和更高版本中都包含 SQL Server Express LocalDB。  

## <a name="see-also"></a>请参阅

- [入门](getting-started.md)
