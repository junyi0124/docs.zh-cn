---
title: DbProviderFactories
ms.date: 03/30/2017
ms.assetid: 2a8e2640-3a49-42a1-a3a9-b43026907ae1
ms.openlocfilehash: b5f2dbb687348afac98247cb21bae831dea26bfe
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183307"
---
# <a name="dbproviderfactories"></a>DbProviderFactories

<xref:System.Data.Common> 命名空间提供用于创建与特定数据源一起使用的 <xref:System.Data.Common.DbProviderFactory> 实例的类。 当创建 <xref:System.Data.Common.DbProviderFactory> 实例并向其传递有关数据提供程序的信息时，`DbProviderFactory` 可以根据为其提供的信息确定要返回的正确的强类型连接对象。  
  
 从 .NET Framework 4 开始，machine.config 文件中不再列出诸如 <xref:System.Data.Odbc>、<xref:System.Data.OleDb>、<xref:System.Data.SqlClient> 和 <xref:System.Data.OracleClient> 等数据提供程序，但会继续列出自定义提供程序。  
  
## <a name="in-this-section"></a>本节内容  

 [工厂模型概述](factory-model-overview.md)  
 提供工厂设计样式和编程接口概述。  
  
 [获取 DbProviderFactory](obtaining-a-dbproviderfactory.md)  
 演示如何列出安装的数据提供程序以及如何从 <xref:System.Data.Common.DbConnection> 创建 `DbProviderFactory`。  
  
 [DbConnection、DbCommand 和 DbException](dbconnection-dbcommand-and-dbexception.md)  
 演示如何创建 <xref:System.Data.Common.DbCommand> 和 <xref:System.Data.Common.DbDataReader>，以及如何使用 <xref:System.Data.Common.DbException> 处理数据错误。  
  
 [使用 DbDataAdapter 修改数据](modifying-data-with-a-dbdataadapter.md)  
 演示如何使用 <xref:System.Data.Common.DbCommandBuilder> 和 <xref:System.Data.Common.DbDataAdapter> 检索并修改数据。  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [ADO.NET 概述](ado-net-overview.md)
