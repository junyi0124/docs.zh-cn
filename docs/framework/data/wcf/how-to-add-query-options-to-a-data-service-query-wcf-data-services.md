---
title: 如何：将查询选项添加到数据服务查询（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- querying the data service [WCF Data Services]
- WCF Data Services, querying
- WCF Data Services, accessing data
ms.assetid: e4258526-557e-4e96-91e1-2175400c7c8f
ms.openlocfilehash: 76d2b1ff3cbb5440bfed62ba840e646cf68de025
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180720"
---
# <a name="how-to-add-query-options-to-a-data-service-query-wcf-data-services"></a>如何：将查询选项添加到数据服务查询（WCF 数据服务）

WCF 数据服务使你能够使用生成的客户端数据服务类从基于 .NET Framework 的客户端应用程序查询数据服务。 执行此操作的最简单方法是编写一个包含所需查询选项的语言集成查询 (LINQ) 查询表达式。 还可以调用一系列 LINQ 查询方法来编写等效的查询。 最后，可以使用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法向查询添加查询选项。 在上述每种情况下，客户端生成的 URI 都包含应用了选定查询选项的被请求实体集。 有关详细信息，请参阅 [查询数据服务](querying-the-data-service-wcf-data-services.md)。  
  
 本主题中的示例使用罗斯文示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。  
  
## <a name="example"></a>示例  

 下面的示例演示如何编写 LINQ 查询表达式，该表达式仅返回运费成本超过 30 美元的订单，并根据发货日期按降序顺序对这些结果进行排序。  
  
 [!code-csharp[Astoria Northwind Client#AddQueryOptionsLinq](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinq)]
 [!code-vb[Astoria Northwind Client#AddQueryOptionsLinq](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinq)]  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 LINQ 查询方法编写等效于上述查询的 LINQ 查询。  
  
 [!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqExpression](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqexpression)]
 [!code-vb[Astoria Northwind Client#AddQueryOptionsLinqExpression](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqexpression)]  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法创建等效于上述示例的 <xref:System.Data.Services.Client.DataServiceQuery%601>。  
  
 [!code-csharp[Astoria Northwind Client#AddQueryOptions](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptions)]
 [!code-vb[Astoria Northwind Client#AddQueryOptions](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptions)]  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 `$orderby` 查询选项按运费属性对返回的订单对象进行筛选和排序。  
  
 [!code-csharp[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#orderwithfilter)]
 [!code-vb[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#orderwithfilter)]  
  
## <a name="see-also"></a>请参阅

- [查询数据服务](querying-the-data-service-wcf-data-services.md)
- [如何：投影查询结果](how-to-project-query-results-wcf-data-services.md)
