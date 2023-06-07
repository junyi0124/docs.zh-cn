---
title: 获取 DbProviderFactory
description: 了解如何从 DbProviderFactories 类获取 DbProviderFactory，以使用 .NET Framework 中的特定数据源。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.openlocfilehash: 0bf95f1773c832a4666c1b33336f963b674052ef
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150734"
---
# <a name="obtaining-a-dbproviderfactory"></a>获取 DbProviderFactory

获取 <xref:System.Data.Common.DbProviderFactory> 的过程涉及将有关数据提供程序的信息传递给 <xref:System.Data.Common.DbProviderFactories> 类。 <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> 方法将基于此信息创建一个强类型提供程序工厂。 例如，若要创建 <xref:System.Data.SqlClient.SqlClientFactory>，可以向 `GetFactory` 传递一个将提供程序名称指定为“System.Data.SqlClient”的字符串。 `GetFactory` 的其他重载采用 <xref:System.Data.DataRow>。 创建该提供程序工厂后，可以使用其方法创建其他对象。 `SqlClientFactory` 的部分方法包括 <xref:System.Data.SqlClient.SqlClientFactory.CreateConnection%2A>、<xref:System.Data.SqlClient.SqlClientFactory.CreateCommand%2A> 和 <xref:System.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>。  
  
> [!NOTE]
> .NET Framework <xref:System.Data.OracleClient.OracleClientFactory>、<xref:System.Data.Odbc.OdbcFactory> 和 <xref:System.Data.OleDb.OleDbFactory> 类也提供类似功能。  
  
## <a name="registering-dbproviderfactories"></a>注册 DbProviderFactory  

 支持基于工厂的类的每个 .NET Framework 数据提供程序在本地计算机上的**machine.config**文件的**DbProviderFactories**节中注册配置信息。 下面的配置文件片断演示 <xref:System.Data.SqlClient> 的语法和格式。  
  
```xml  
<system.data>  
  <DbProviderFactories>  
    <add name="SqlClient Data Provider"  
     invariant="System.Data.SqlClient"
     description=".Net Framework Data Provider for SqlServer"
     type="System.Data.SqlClient.SqlClientFactory, System.Data,
     Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"  
    />  
  </DbProviderFactories>  
</system.data>  
```  
  
 **固定**属性标识基础数据提供程序。 在创建新工厂时也使用这种由三部分组成的命名语法，并用于标识应用程序配置文件中的提供程序，以便在运行时能够检索提供程序名称及其关联的连接字符串。  
  
## <a name="retrieving-provider-information"></a>检索提供程序信息  

 使用 <xref:System.Data.Common.DbProviderFactories.GetFactoryClasses%2A> 方法可以检索有关安装在本地计算机上的所有数据提供程序的信息。 它返回一个 <xref:System.Data.DataTable> 名为 **DbProviderFactories** 的，其中包含下表中所述的列。  
  
|列序号|列名称|示例输出|描述|  
|--------------------|-----------------|--------------------|-----------------|  
|0|**名称**|SqlClient Data Provider|数据提供程序的可读名称|  
|1|**说明**|.Net Framework Data Provider for SqlServer|数据提供程序的可读说明|  
|2| InvariantName****|System.Data.SqlClient|数据提供程序的名称，可用于以编程方式引用该数据提供程序|  
|3| AssemblyQualifiedName****|System.Data.SqlClient.SqlClientFactory, System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089|工厂类的完全限定名称，其中包含实例化对象所需的足够信息|  
  
 使用此 `DataTable`，用户可以在运行时选择 <xref:System.Data.DataRow>。 然后，可以将所选的 `DataRow` 传递给 <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> 方法以创建强类型 <xref:System.Data.Common.DbProviderFactory>。 可以将所选的 <xref:System.Data.DataRow> 传递给 `GetFactory` 方法以创建需要的 `DbProviderFactory` 对象。  
  
## <a name="listing-the-installed-provider-factory-classes"></a>列出已安装的提供程序工厂类  

 此示例演示如何使用 <xref:System.Data.Common.DbProviderFactories.GetFactoryClasses%2A> 方法返回一个包含有关已安装提供程序信息的 <xref:System.Data.DataTable>。 代码遍历 `DataTable` 中的每一行，在控制台窗口中显示每个已安装的提供程序的信息。  
  
 [!code-csharp[DataWorks DbProviderFactories#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories/VB/source.vb#1)]  
  
## <a name="using-application-configuration-files-to-store-factory-information"></a>使用应用程序配置文件存储工厂信息  

 用于使用工厂的设计模式需要在应用程序配置文件中存储提供程序和连接字符串信息，如 Windows 应用程序 **app.config** ，并 **web.config** ASP.NET 应用程序。  
  
 下面的配置文件片段演示如何保存两个命名连接字符串：用于连接到 SQL Server 中 Northwind 数据库的“NorthwindSQL”和用于连接到 Access/Jet 中 Northwind 数据库的“NorthwindAccess”。 **固定**名称用于**providerName**特性。  
  
```xml  
<configuration>  
  <connectionStrings>  
    <clear/>  
    <add name="NorthwindSQL"
     providerName="System.Data.SqlClient"
     connectionString=  
     "Data Source=MSSQL1;Initial Catalog=Northwind;Integrated Security=true"  
    />  
  
    <add name="NorthwindAccess"
     providerName="System.Data.OleDb"
     connectionString=  
     "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\Data\Northwind.mdb;"  
    />  
  </connectionStrings>  
</configuration>  
```  
  
### <a name="retrieving-a-connection-string-by-provider-name"></a>按提供程序名称检索连接字符串  

 若要创建提供程序工厂，必须提供连接字符串和提供程序名称。 此示例演示如何通过以固定*格式 "system.string" 传递*提供程序名称来从应用程序配置文件中检索连接字符串。 代码循环访问 <xref:System.Configuration.ConnectionStringSettingsCollection>。 成功时代码返回 <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>；否则返回 `null`（在 Visual Basic 中为 `Nothing`）。 如果提供程序有多项，则返回找到的第一项。 有关从配置文件中检索连接字符串的详细信息和示例，请参阅 [连接字符串和配置文件](connection-strings-and-configuration-files.md)。  
  
> [!NOTE]
> 若要此代码正确运行，需要引用 `System.Configuration.dll`。  
  
 [!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider/CS/source.cs#1)]
 [!code-vb[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider/VB/source.vb#1)]  
  
## <a name="creating-the-dbproviderfactory-and-dbconnection"></a>创建 DbProviderFactory 和 DbConnection  

 此示例演示如何 <xref:System.Data.Common.DbProviderFactory> 通过以 "system.string <xref:System.Data.Common.DbConnection> " 格式和连接字符串的形式传递提供程序名称*System.Data.ProviderName*来创建和对象。 成功时返回 `DbConnection` 对象；出错时返回 `null`（在 Visual Basic 中为 `Nothing`）。  
  
 代码通过调用 `DbProviderFactory` 获取 <xref:System.Data.Common.DbProviderFactories.GetFactory%2A>。 然后，<xref:System.Data.Common.DbProviderFactory.CreateConnection%2A> 方法创建 <xref:System.Data.Common.DbConnection> 对象并将 <xref:System.Data.Common.DbConnection.ConnectionString%2A> 属性设置为连接字符串。  
  
 [!code-csharp[DataWorks DbProviderFactories.GetFactory#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.GetFactory/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories.GetFactory#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.GetFactory/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [DbProviderFactories](dbproviderfactories.md)
- [连接字符串](connection-strings.md)
- [使用配置类](/previous-versions/aspnet/ms228063(v=vs.100))
- [ADO.NET 概述](ado-net-overview.md)
