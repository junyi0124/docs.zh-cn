---
title: 如何：使用 LINQ to SQL 数据源创建数据服务（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, LINQ to SQL
- WCF Data Services, providers
ms.assetid: 3b01c2fd-8c6e-4bf5-b38f-9e61bdc3c328
ms.openlocfilehash: cde5b9903a1fd164ce106a6a408ac4bb79976642
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802280"
---
# <a name="how-to-create-a-data-service-using-a-linq-to-sql-data-source-wcf-data-services"></a>如何：使用 LINQ to SQL 数据源创建数据服务（WCF 数据服务）

WCF 数据服务将实体数据作为数据服务公开。 反射提供程序允许您定义基于任何类的数据模型，该类公开返回 <xref:System.Linq.IQueryable%601> 实现的成员。 为了能够更新数据源中的数据，这些类还必须实现 <xref:System.Data.Services.IUpdatable> 接口。 有关详细信息，请参阅[数据服务提供程序](data-services-providers-wcf-data-services.md)。 本主题演示如何创建通过使用反射提供程序来访问 Northwind 示例数据库的 LINQ to SQL 类，以及如何创建基于这些数据类的数据服务。

## <a name="to-add-linq-to-sql-classes-to-a-project"></a>向项目中添加 LINQ to SQL 类

1. C#在 Visual Basic 或应用程序中，在 "**项目**" 菜单上，单击 "**添加** > **新项**"。

2. 单击 " **LINQ to SQL 类**" 模板。

3. 将名称更改为 " **Northwind**"。

4. 单击 **添加**。

     此时，Northwind.dbml 文件将随即添加到项目中且对象关系设计器（O/R 设计器）将打开。

5. 在**服务器**/**数据库资源管理器**的 "Northwind" 下，展开 "**表**"，然后将 `Customers` 表拖到对象关系设计器（O/R 设计器）上。

     此时将随即创建一个 `Customer` 实体类并显示在设计图面上。

6. 对 `Orders`、`Order_Details` 和 `Products` 表重复步骤 6。

7. 右键单击表示 LINQ to SQL 类的新 .dbml 文件，然后单击 "**查看代码**"。

     这将创建一个名为 Northwind.cs 的新代码隐藏页，其中包含从 <xref:System.Data.Linq.DataContext> 类（在此示例中为 `NorthwindDataContext`）继承的类的分部类定义。

8. 用下列代码替换 Northwind.cs 代码文件的内容。 此代码通过扩展 <xref:System.Data.Linq.DataContext> 以及 LINQ to SQL 生成的数据类来实现反射提供程序：

     [!code-csharp[Astoria Linq Provider#Linq2SqlProvider](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_linq_provider/cs/northwind.cs#linq2sqlprovider)]
     [!code-vb[Astoria Linq Provider#Linq2SqlProvider](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_linq_provider/vb/northwind.vb#linq2sqlprovider)]

### <a name="to-create-a-data-service-by-using-a-linq-to-sql-based-data-model"></a>使用基于 LINQ to SQL 的数据模型创建数据服务

1. 在**解决方案资源管理器**中，右键单击 ASP.NET 项目的名称，然后单击 "**添加** > **新项**"。

2. 在 "**添加新项**" 对话框中，从 " **Web** " 类别中选择 " **WCF 数据服务**" 模板。

   ![Visual Studio 2015 中的 WCF 数据服务项模板](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > **WCF 数据服务**模板在 visual studio 2015 中提供，但在 visual studio 2017 或更高版本中不可用。

3. 提供服务的名称，然后单击 **"确定"** 。

     将为新服务创建 XML 标记和代码文件。 默认情况下，代码编辑器窗口将打开。

4. 在数据服务的代码中，用数据模型的实体容器的类型（在此示例中为 `/* TODO: put your data source class name here */`）替换定义数据服务的类定义中的注释 `NorthwindDataContext`。

5. 在数据服务的代码中，用下列代码替换 `InitializeService` 函数中的占位符代码：

     [!code-csharp[Astoria Linq Provider#EnableAccess](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_linq_provider/cs/northwind.svc.cs#enableaccess)]
     [!code-vb[Astoria Linq Provider#EnableAccess](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_linq_provider/vb/northwind.svc.vb#enableaccess)]

     这使得授权客户端能够访问指定的三个实体集的资源。

6. 若要使用 Web 浏览器测试 Northwind 数据服务，请按照主题[从 Web 浏览器访问该服务](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)中的说明进行操作。

## <a name="see-also"></a>另请参阅

- [如何：使用 ADO.NET 实体框架数据源创建数据服务](create-a-data-service-using-an-adonet-ef-data-wcf.md)
- [如何：使用反射提供程序创建数据服务](create-a-data-service-using-rp-wcf-data-services.md)
- [数据服务提供程序](data-services-providers-wcf-data-services.md)
