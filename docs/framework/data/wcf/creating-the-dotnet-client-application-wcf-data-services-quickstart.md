---
title: 创建 .NET Framework 客户端应用程序（WCF 数据服务快速入门）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 41ade767-eeab-437d-9121-9797e8fb8045
ms.openlocfilehash: 4beaba24e42b15ebc45ece6e5319a2b14df54ab6
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975383"
---
# <a name="creating-the-net-framework-client-application-wcf-data-services-quickstart"></a>创建 .NET Framework 客户端应用程序（WCF 数据服务快速入门）

这是 WCF 数据服务快速入门的最后一任务。 在此任务中，你将向解决方案添加一个控制台应用程序，将对 Open Data Protocol （OData）源的引用添加到此新的客户端应用程序，并使用生成的客户端数据服务类和客户端从客户端应用程序访问 OData 源库.

> [!NOTE]
> 访问数据源不需要基于 .NET Framework 的客户端应用程序。 使用 OData 源的任何应用程序组件都可以访问该数据服务。 有关详细信息，请参阅[在客户端应用程序中使用数据服务](using-a-data-service-in-a-client-application-wcf-data-services.md)。

## <a name="to-create-the-client-application-by-using-visual-studio"></a>使用 Visual Studio 创建客户端应用程序

1. 在**解决方案资源管理器**中，右键单击解决方案，单击 "**添加**"，然后单击 "**新建项目**"。

2. 在左窗格中，选择 **"已安装**> [**视觉对象C#** 或**Visual Basic**]" > **Windows 桌面**"，然后选择" **WPF 应用程序**"模板。

3. 输入项目名称 `NorthwindClient`，然后单击 **"确定"** 。

4. 打开文件 MainWindow.xaml 并用下列代码替换 XAML 代码：

     [!code-xaml[Astoria Quickstart Client#Window1Xaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml#window1xaml)]

## <a name="to-add-a-data-service-reference-to-the-project"></a>在项目中添加数据服务引用

1. 在**解决方案资源管理器**中，右键单击 NorthwindClient 项目，单击 "**添加** > **服务引用**"，然后单击 "**发现**"。

     这将显示在第一个任务中创建的 Northwind 数据服务。

2. 在 "**命名空间**" 文本框中，键入 `Northwind`，然后单击 **"确定"** 。

     这将在项目中添加一个新的代码文件，其中包含用于作为对象访问数据服务资源并与其交互的数据类。 这些数据类是在命名空间 `NorthwindClient.Northwind` 中创建的。

## <a name="to-access-data-service-data-in-the-wpf-application"></a>在 WPF 应用程序中访问数据服务数据

1. 在 " **NorthwindClient**" 下的 "**解决方案资源管理器**中，右键单击项目，然后单击"**添加引用**"。

2. 在 "**添加引用**" 对话框中，单击 " **.net** " 选项卡，选择 ""，然后单击 **"确定**"。

3. 在 " **NorthwindClient**" 下的 "**解决方案资源管理器**中，打开 mainwindow.xaml 文件的代码页，然后添加以下 `using` 语句（`Imports` Visual Basic 中）。

    [!code-csharp[Astoria Quickstart Client#Using](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_client/cs/window1.xaml.cs#using)]
    [!code-vb[Astoria Quickstart Client#Using](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml.vb#using)]

4. 将用于查询数据服务并将结果绑定到 <xref:System.Data.Services.Client.DataServiceCollection%601> 的下列代码插入到 `MainWindow` 类：

    > [!NOTE]
    > 必须用承载 Northwind 数据服务的实例的服务器和端口替换主机名 `localhost:12345`。

     [!code-csharp[Astoria Quickstart Client#QueryCode](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_client/cs/window1.xaml.cs#querycode)]
     [!code-vb[Astoria Quickstart Client#QueryCode](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml.vb#querycode)]

5. 将用于保存更改的下列代码插入到 `MainWindow` 类：

     [!code-csharp[Astoria Quickstart Client#SaveChanges](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_client/cs/window1.xaml.cs#savechanges)]
     [!code-vb[Astoria Quickstart Client#SaveChanges](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml.vb#savechanges)]

## <a name="to-build-and-run-the-northwindclient-application"></a>生成并运行 NorthwindClient 应用程序

1. 在**解决方案资源管理器**中，右键单击 NorthwindClient 项目，然后选择 "**设为启动项目**"。

2. 按**F5**启动应用程序。

     这将生成解决方案并启动客户端应用程序。 将从服务请求数据并将其显示在控制台中。

3. 编辑数据网格的 "**数量**" 列中的值，然后单击 "**保存**"。

     将更改保存到数据服务。

    > [!NOTE]
    > 此版本的 NorthwindClient 应用程序不支持添加和删除实体。

## <a name="next-steps"></a>后续步骤

你已成功创建了访问示例 Northwind OData 源的客户端应用程序。 你还完成了 WCF 数据服务快速入门！

有关从 .NET Framework 应用程序访问 OData 源的详细信息，请参阅[WCF 数据服务客户端库](wcf-data-services-client-library.md)。

## <a name="see-also"></a>请参阅

- [入门](getting-started-with-wcf-data-services.md)
- [资源](wcf-data-services-resources.md)
