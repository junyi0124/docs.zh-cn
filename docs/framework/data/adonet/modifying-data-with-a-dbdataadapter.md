---
title: 使用 DbDataAdapter 修改数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e35c7f9e-648b-4fcc-9361-d365c3e42c9a
ms.openlocfilehash: 5272a53ae0b3ac1888d01dc2a59778c6c7231619
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150760"
---
# <a name="modifying-data-with-a-dbdataadapter"></a>使用 DbDataAdapter 修改数据

<xref:System.Data.Common.DbProviderFactory.CreateDataAdapter%2A> 对象的 <xref:System.Data.Common.DbProviderFactory> 方法为您提供 <xref:System.Data.Common.DbDataAdapter> 对象，该对象强类型化为创建工厂时指定的基础数据提供程序。 然后您可以使用 <xref:System.Data.Common.DbCommandBuilder> 创建命令，以插入、更新和删除数据源的 <xref:System.Data.DataSet> 中的数据。  
  
## <a name="retrieving-data-with-a-dbdataadapter"></a>使用 DbDataAdapter 检索数据  

 此示例演示如何基于提供程序名称和连接字符串来创建强类型的 `DbDataAdapter`。 代码使用 <xref:System.Data.Common.DbProviderFactory.CreateConnection%2A> 的 <xref:System.Data.Common.DbProviderFactory> 方法创建 <xref:System.Data.Common.DbConnection>。 接下来，代码使用 <xref:System.Data.Common.DbProviderFactory.CreateCommand%2A> 方法创建 <xref:System.Data.Common.DbCommand> 以便通过设置其 `CommandText` 和 `Connection` 属性来选择数据。 最后，代码使用 <xref:System.Data.Common.DbDataAdapter> 方法创建 <xref:System.Data.Common.DbProviderFactory.CreateDataAdapter%2A> 对象并设置其 `SelectCommand` 属性。 `Fill` 的 `DbDataAdapter` 方法将数据加载到 <xref:System.Data.DataTable> 中。  
  
 [!code-csharp[DataWorks DbProviderFactories.DbDataAdapter#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbDataAdapter/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories.DbDataAdapter#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbDataAdapter/VB/source.vb#1)]  
  
## <a name="modifying-data-with-a-dbdataadapter"></a>使用 DbDataAdapter 修改数据  

 此示例演示如何通过使用 `DataTable` 生成更新数据源的数据所需的命令，从而使用 <xref:System.Data.Common.DbDataAdapter> 来修改 <xref:System.Data.Common.DbCommandBuilder> 中的数据。 <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> 的 `DbDataAdapter` 设置为从 Customers 表检索 CustomerID 和 CompanyName。 <xref:System.Data.Common.DbCommandBuilder.GetInsertCommand%2A> 方法用于设置 <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A> 属性，<xref:System.Data.Common.DbCommandBuilder.GetUpdateCommand%2A> 方法用于设置 <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> 属性，而 <xref:System.Data.Common.DbCommandBuilder.GetDeleteCommand%2A> 方法用于设置 <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> 属性。 代码向 Customers 表中添加一新行并更新数据源。 然后，代码通过搜索 CustomerID（为 Customers 表定义的主键）定位添加的行。 代码更改 CompanyName 并更新数据源。 最后，代码删除该行。  
  
 [!code-csharp[DataWorks DbProviderFactories.DbDataAdapterModify#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbDataAdapterModify/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories.DbDataAdapterModify#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbDataAdapterModify/VB/source.vb#1)]  
  
## <a name="handling-parameters"></a>处理参数  

 .NET Framework 数据提供程序以不同方式处理命名和指定参数和参数占位符。 此语法针对特定数据源量身定制，如下表所述。  
  
|数据提供程序|参数命名语法|  
|-------------------|-----------------------------|  
|`SqlClient`|以 `@`*参数名*格式使用命名参数。|  
|`OracleClient`|以 `:`*参数名* （或 *参数名*）格式使用命名参数。|  
|`OleDb`|使用由问号 (`?`) 指示的位置参数标记。|  
|`Odbc`|使用由问号 (`?`) 指示的位置参数标记。|  
  
 工厂模型对于创建参数化 `DbCommand` 和 `DbDataAdapter` 对象不具有帮助价值。 你需要编写分支代码来创建针对数据提供程序定制的参数。  
  
> [!IMPORTANT]
> 出于安全原因，建议不要通过采用字符串串联的形式构造直接 SQL 语句来完全避免提供程序特定的参数。 使用字符串串联代替参数会使您的应用程序容易受到 SQL 注入攻击。  
  
## <a name="see-also"></a>请参阅

- [DbProviderFactories](dbproviderfactories.md)
- [获取 DbProviderFactory](obtaining-a-dbproviderfactory.md)
- [DbConnection、DbCommand 和 DbException](dbconnection-dbcommand-and-dbexception.md)
- [ADO.NET 概述](ado-net-overview.md)
