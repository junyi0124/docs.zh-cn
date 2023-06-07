---
title: 工厂模型概述
ms.date: 03/30/2017
ms.assetid: b5dc81c4-7554-44b9-b513-769bd61e2e7b
ms.openlocfilehash: 7cee3966ab3a37d2dbc6dd0ea9ab26b485ef63fd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156389"
---
# <a name="factory-model-overview"></a>工厂模型概述

ADO.NET 2.0 在 <xref:System.Data.Common> 命名空间中引入了新基类。 基类为抽象类，这意味着它们不能直接实例化。 这些基类包括 <xref:System.Data.Common.DbConnection>、<xref:System.Data.Common.DbCommand> 和 <xref:System.Data.Common.DbDataAdapter>，它们由 .NET Framework 数据提供程序（如 <xref:System.Data.SqlClient> 和 <xref:System.Data.OleDb>）共享。 添加基类简化了向 .NET Framework 数据提供程序添加功能的过程，不再需要创建新接口。  
  
 ADO.NET 2.0 中还引入了一些抽象基类，使开发人员能够编写不依赖于特定数据提供程序的一般数据访问代码。  
  
## <a name="the-factory-design-pattern"></a>工厂设计模式  

 编写独立于提供程序的代码的编程模型基于“工厂”设计模式的使用，此模式使用单个 API 跨多个提供程序访问数据库。 此模式的命名非常恰当，因为它需单独使用专用的对象来创建其他对象，与实际的工厂非常类似。 有关工厂设计模式的更详细说明，请参阅 [在 ASP.NET 2.0 和 ADO.NET 2.0 中编写通用数据访问代码](/previous-versions/dotnet/articles/ms971499(v=msdn.10))。
  
 从 ADO.NET 2.0 开始，<xref:System.Data.Common.DbProviderFactories> 类提供 `static`（或 Visual Basic 中的 `Shared`）方法以用于创建 <xref:System.Data.Common.DbProviderFactory> 实例。 该实例随后会基于提供程序信息和运行时提供的连接字符串返回正确的强类型对象。  
  
## <a name="see-also"></a>请参阅

- [获取 DbProviderFactory](obtaining-a-dbproviderfactory.md)
- [DbConnection、DbCommand 和 DbException](dbconnection-dbcommand-and-dbexception.md)
- [使用 DbDataAdapter 修改数据](modifying-data-with-a-dbdataadapter.md)
- [ADO.NET 概述](ado-net-overview.md)
