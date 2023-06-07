---
title: 迁移注意事项（实体框架）
ms.date: 03/30/2017
ms.assetid: c85b6fe8-cc32-4642-8f0a-dc0e5a695936
ms.openlocfilehash: d5151f0215cde1c5e35cc87c3cd667e16a23a1df
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175468"
---
# <a name="migration-considerations-entity-framework"></a>迁移注意事项（实体框架）

ADO.NET 实体框架为现有应用程序提供了几个优点。 其中最重要的益处之一在于能够使用概念模型将应用程序使用的数据结构与数据源中的架构分离。 这样，日后便可轻松地对存储模型或数据源本身进行更改，而无需对应用程序进行补偿更改。 有关使用实体框架的好处的详细信息，请参阅 [实体框架概述](overview.md) 和 [实体数据模型](../entity-data-model.md)。  
  
 若要利用实体框架的优势，可以将现有应用程序迁移到实体框架中。 某些任务对于所有迁移的应用程序是通用的。 这些常见任务包括升级应用程序以使用从3.5 版 Service Pack 1 (SP1) 、定义模型和映射和配置实体框架开始 .NET Framework。 将应用程序迁移到实体框架时，还需要考虑其他注意事项。 相关注意事项取决于要迁移的应用程序的类型及具体功能。 本主题提供的信息可帮助您为现有应用程序选择最佳的升级途径。  
  
## <a name="general-migration-considerations"></a>有关迁移的一般注意事项  

 将任何应用程序迁移到实体框架时，请注意以下事项：  
  
- 只要应用程序使用的数据源的数据提供程序支持实体框架，任何使用从版本 3.5 SP1 开始的 .NET Framework 的应用程序都可以迁移到实体框架。  
  
- 即使某个数据源提供程序支持实体框架，实体框架也可能不支持该提供程序的所有功能。  
  
- 对于大型或复杂的应用程序，不需要一次性地将整个应用程序迁移到实体框架。 但是，在数据源发生更改时，仍然需要更改应用程序中不使用实体框架的任何部分。  
  
- 实体框架使用的数据提供程序连接可以与应用程序的其他部分共享，因为实体框架使用 ADO.NET 数据提供程序访问数据源。 例如，实体框架使用 SqlClient 提供程序访问 SQL Server 数据库。 有关详细信息，请参阅 [用于 Entity Framework 的 EntityClient 提供程序](entityclient-provider-for-the-entity-framework.md)。  
  
## <a name="common-migration-tasks"></a>通用迁移任务  

 将现有应用程序迁移到实体框架的路径取决于应用程序的类型和现有的数据访问策略。 但是，在将现有应用程序迁移到实体框架时，必须始终执行以下任务。  
  
> [!NOTE]
> 从 Visual Studio 2008 开始使用实体数据模型工具时，将自动执行所有这些任务。 有关详细信息，请参阅 [如何：使用实体数据模型向导](/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))。  
  
1. 升级应用程序。  
  
     使用早期版本的 Visual Studio 和 .NET Framework 创建的项目必须升级为使用 Visual Studio 2008 SP1 和从版本 3.5 SP1 开始 .NET Framework。  
  
2. 定义模型和映射。  
  
     模型和映射文件定义概念模型中的实体、数据源中的结构（如表、存储过程和视图）以及实体与数据源结构之间的映射。 有关详细信息，请参阅 [如何：手动定义模型和映射文件](/previous-versions/dotnet/netframework-4.0/bb399785(v=vs.100))。  
  
     在存储模型中定义的类型必须与数据源中对象的名称相匹配。 如果现有应用程序将数据作为对象公开，则必须确保在概念模型中定义的实体和属性与这些现有数据类和属性的名称相匹配。 有关详细信息，请参阅 [如何：自定义建模和映射文件以使用自定义对象](/previous-versions/dotnet/netframework-4.0/bb738625(v=vs.100))。  
  
    > [!NOTE]
    > 可以使用实体数据模型设计器重命名概念模型中的实体以匹配现有对象。 有关详细信息，请参阅 [实体数据模型设计器](/previous-versions/dotnet/netframework-4.0/cc716685(v=vs.100))。  
  
3. 定义连接字符串。  
  
     实体框架在针对概念模型执行查询时使用特殊格式的连接字符串。 此连接字符串包装有关模型和映射文件和到数据源的连接的信息。 有关详细信息，请参阅 [如何：定义连接字符串](how-to-define-the-connection-string.md)。  
  
4. 配置 Visual Studio 项目。  
  
     必须将对实体框架程序集和模型和映射文件的引用添加到 Visual Studio 项目中。 可以将这些映射文件添加到项目中，以确保这些文件随应用程序一起部署在连接字符串中所指示的位置。 有关详细信息，请参阅 [如何：手动配置实体框架项目](/previous-versions/dotnet/netframework-4.0/bb738546(v=vs.100))。  
  
## <a name="considerations-for-applications-with-existing-objects"></a>有关包含现有对象的应用程序的注意事项  

 从 .NET Framework 4 开始，实体框架支持 (POCO) 的 "纯旧" CLR 对象（也称为持久性未知对象）。 在大多数情况下，你的现有对象可通过进行少量更改来处理实体框架。 有关详细信息，请参阅使用 [POCO 实体](/previous-versions/dotnet/netframework-4.0/dd456853(v=vs.100))。 你还可以将应用程序迁移到实体框架，并使用实体框架工具生成的数据类。 有关详细信息，请参阅 [如何：使用实体数据模型向导](/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))。  
  
## <a name="considerations-for-applications-that-use-adonet-providers"></a>有关使用 ADO.NET 提供程序的应用程序的注意事项  

 使用 ADO.NET 提供程序（如 SqlClient），您可以查询数据源以返回表格数据。 还可以将数据加载到 ADO.NET 数据集中。 以下列表描述了升级使用现有 ADO.NET 提供程序的应用程序时的注意事项：  
  
- 使用数据读取器显示表格格式数据。  

  可以考虑 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 使用 EntityClient 提供程序执行查询并枚举返回的 <xref:System.Data.EntityClient.EntityDataReader> 对象。 仅当应用程序使用数据读取器显示表格格式数据，且不需要实体框架提供的工具将数据具体化为对象、跟踪更改和进行更新时，才执行此操作。 可以继续使用对数据源进行更新的现有数据访问代码，但可以使用从 <xref:System.Data.EntityClient.EntityConnection.StoreConnection%2A> 的 <xref:System.Data.EntityClient.EntityConnection> 属性访问的现有连接。 有关详细信息，请参阅 [用于 Entity Framework 的 EntityClient 提供程序](entityclient-provider-for-the-entity-framework.md)。  
  
- 使用数据集。  

  实体框架提供了许多与数据集提供的功能相同的功能，包括内存中持久性、更改跟踪、数据绑定以及将对象序列化为 XML 数据。 有关详细信息，请参阅使用 [对象](working-with-objects.md)。  
  
  如果实体框架未提供应用程序所需的数据集功能，仍然可以通过使用 LINQ to DataSet 来利用 LINQ 查询的优点。 有关详细信息，请参阅 [LINQ to DataSet](../linq-to-dataset.md)。  
  
## <a name="considerations-for-applications-that-bind-data-to-controls"></a>有关将数据绑定到控件的应用程序的注意事项  

 .NET Framework 允许您在数据源（例如数据集或 ASP.NET 数据源控件）中封装数据，然后将用户界面元素绑定到这些数据控件。 下表介绍将控件绑定到实体框架数据时的注意事项。  
  
- 将数据绑定到控件。  

  在查询概念模型时，实体框架以实体类型实例的对象的形式返回数据。 这些对象可以直接绑定到控件，此绑定支持更新。 这意味着对控件中的数据（如中的行）所做的更改 <xref:System.Windows.Forms.DataGridView> 会在调用方法时自动保存到数据库 <xref:System.Data.Objects.ObjectContext.SaveChanges%2A> 。  
  
  如果应用程序对查询结果进行枚举，以在 <xref:System.Windows.Forms.DataGridView> 或支持数据绑定的其他类型的控件中显示数据，则可以修改应用程序以将该控件绑定到 <xref:System.Data.Objects.ObjectQuery%601> 的结果。  
  
  有关详细信息，请参阅 [将对象绑定到控件](/previous-versions/dotnet/netframework-4.0/bb738469(v=vs.100))。  
  
- ASP.NET 数据源控件。  

  实体框架包含用于简化 ASP.NET Web 应用程序中的数据绑定的数据源控件。 有关详细信息，请参阅 [EntityDataSource Web 服务器控件概述](/previous-versions/aspnet/cc488502(v=vs.100))。  
  
## <a name="other-considerations"></a>其他注意事项  

 以下是在将特定类型的应用程序迁移到实体框架时可能需要考虑的一些注意事项。  
  
- 公开数据服务的应用程序。  

  基于 Windows Communication Foundation (WCF) 的 Web 服务和应用程序使用 XML 请求/响应消息格式公开基础数据源中的数据。 实体框架支持通过使用二进制、XML 或 WCF 数据协定序列化来序列化实体对象。 二进制和 WCF 序列化都支持对象图的整体序列化。 有关详细信息，请参阅 [生成 N 层应用程序](/previous-versions/dotnet/netframework-4.0/bb896304(v=vs.100))。  
  
- 使用 XML 数据的应用程序。  

  对象序列化允许您创建实体框架数据服务。 这些服务为使用 XML 数据的应用程序（如基于 AJAX 的 Internet 应用程序）提供数据。 在这些情况下，请考虑使用 WCF 数据服务。 这些数据服务基于实体数据模型，并使用标准具象状态传输 (REST) HTTP 操作（如 GET、PUT 和 POST）提供对实体数据的动态访问。 有关详细信息，请参阅 [WCF Data Services 4.5](../../wcf/index.md)。  
  
  实体框架不支持本机 XML 数据类型。 这意味着在将实体映射到包含 XML 列的表时，该 XML 列的等效实体属性是一个字符串。 对象可以断开连接，并序列化为 XML。 有关详细信息，请参阅 [序列化对象](/previous-versions/dotnet/netframework-4.0/bb738446(v=vs.100))。  
  
  如果应用程序需要查询 XML 数据，则仍然可以通过使用 LINQ to XML 来利用 LINQ 查询的优点。 有关详细信息，请参阅 [LINQ to XML (c # ) ](../../../../standard/linq/linq-xml-overview.md) 或 [LINQ to XML (](../../../../standard/linq/linq-xml-overview.md)Visual Basic) 。  
  
- 维护状态的应用程序。  

  ASP.NET Web 应用程序必须经常维护网页或用户会话的状态。 实例中的对象 <xref:System.Data.Objects.ObjectContext> 可以存储在客户端视图状态或服务器上的会话状态中，之后检索和重新附加到新的对象上下文。 有关详细信息，请参阅 [附加和分离对象](/previous-versions/dotnet/netframework-4.0/bb896271(v=vs.100))。  
  
## <a name="see-also"></a>请参阅

- [部署注意事项](deployment-considerations.md)
- [实体框架术语](terminology.md)
