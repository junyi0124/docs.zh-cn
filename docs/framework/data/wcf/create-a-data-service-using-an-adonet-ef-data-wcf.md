---
title: 如何：使用 ADO.NET 实体框架数据源创建数据服务（WCF 数据服务）
ms.date: 08/24/2018
helpviewer_keywords:
- WCF Data Services, providers
- WCF Data Services, Entity Framework
ms.assetid: 6d11fec8-0108-42f5-8719-2a7866d04428
ms.openlocfilehash: 1e559488a3260fafe6c211ff47226a258fc1289a
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557692"
---
# <a name="how-to-create-a-data-service-using-an-adonet-entity-framework-data-source-wcf-data-services"></a>如何：使用 ADO.NET 实体框架数据源创建数据服务（WCF 数据服务）

WCF 数据服务将实体数据作为数据服务公开。 当数据源为关系数据库时，NETEntity 框架将提供此实体数据。 本主题介绍如何在基于现有数据库的 Visual Studio Web 应用程序中创建基于实体框架的数据模型，以及如何使用此数据模型创建新的数据服务。

实体框架还提供可以在 Visual Studio 项目外生成实体框架模型的命令行工具。 有关详细信息，请参阅 [如何：使用 EdmGen.exe 生成模型和映射文件](../adonet/ef/how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md)。

## <a name="to-add-an-entity-framework-model-that-is-based-on-an-existing-database-to-an-existing-web-application"></a>将基于现有数据库的实体框架模型添加到现有 Web 应用程序

1. 在 "**项目**" 菜单上，单击 "**添加**  >  **新项**"。

2. 在 " **模板** " 窗格中，单击 " **数据** " 类别，然后选择 " **ADO.NET 实体数据模型**"。

3. 输入模型名称，然后单击 " **添加**"。

     将显示“实体数据模型向导”的第一页。

4. 在 " **选择模型内容** " 对话框中，选择 " **从数据库生成**"。 然后单击“下一步”  。

5. 单击 " **新建连接** " 按钮。

6. 在 " **连接属性** " 对话框中，键入服务器名称，选择身份验证方法，键入数据库名称，然后单击 **"确定"**。

     " **选择您的数据连接** " 对话框将通过数据库连接设置进行更新。

7. 确保选中 " **将实体连接设置保存 App.Config 为：** " 复选框。 然后单击“下一步”  。

8. 在 " **选择数据库对象** " 对话框中，选择计划在数据服务中公开的所有数据库对象。

    > [!NOTE]
    > 数据服务不自动公开数据模型中包含的对象。 它们必须由服务本身显式公开。 有关详细信息，请参阅 [配置数据服务](configuring-the-data-service-wcf-data-services.md)。

9. 单击 **“完成”** 以完成向导。

     这将基于特定数据库创建默认数据模型。 实体框架允许对数据模型进行自定义。 有关详细信息，请参阅 [实体数据模型工具任务](/previous-versions/dotnet/netframework-4.0/bb738480(v=vs.100))。

## <a name="to-create-the-data-service-by-using-the-new-data-model"></a>使用新数据模型创建数据服务

1. 在 Visual Studio 中，打开代表数据模型的 .edmx 文件。

2. 在 **模型浏览器**中，右键单击模型，单击 " **属性**"，然后记下实体容器的名称。

3. 在**解决方案资源管理器**中，右键单击 ASP.NET 项目的名称，然后单击 "**添加**  >  **新项**"。

4. 在 "**添加新项**" 对话框中，选择 " **Web** " 类别中的 " **WCF 数据服务**" 模板。

   ![Visual Studio 2015 中的 WCF 数据服务项模板](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > **WCF 数据服务**模板在 visual studio 2015 中提供，但在 visual studio 2017 或更高版本中不可用。

5. 提供服务的名称，然后单击 **"确定"**。

     Visual Studio 将为新服务创建 XML 标记和代码文件。 默认情况下，代码编辑器窗口将打开。

6. 在数据服务代码中，将用于定义数据服务的类定义中的注释 `/* TODO: put your data source class name here */` 替换为从 <xref:System.Data.Objects.ObjectContext> 类继承且作为数据模型的实体容器的类型，如步骤 2 中所述。

7. 在数据服务代码中，启用经过授权的客户端来访问数据服务公开的实体集。 有关详细信息，请参阅 [创建数据服务](creating-the-data-service.md)。

8. 若要使用 Web 浏览器测试 Northwind 数据服务，请按照主题 [从 Web 浏览器访问该服务](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)中的说明进行操作。

## <a name="see-also"></a>请参阅

- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [数据服务提供程序](data-services-providers-wcf-data-services.md)
- [如何：使用反射提供程序创建数据服务](create-a-data-service-using-rp-wcf-data-services.md)
- [如何：使用 LINQ to SQL 数据源创建数据服务](create-a-data-service-using-linq-to-sql-wcf.md)
