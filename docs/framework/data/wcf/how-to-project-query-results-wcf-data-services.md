---
title: 如何：投影查询结果（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- projection [WCF Data Services]
- WCF Data Services, projection
- query projection [WCF Data Services]
- WCF Data Services, querying
ms.assetid: 474ac625-8770-43ba-8320-d3315ea9530f
ms.openlocfilehash: 8a3a278a8459da073b7ad3cbf8d1fff1d435a18c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175182"
---
# <a name="how-to-project-query-results-wcf-data-services"></a>如何：投影查询结果（WCF 数据服务）

投影提供减少查询返回的数据量的机制，方法是指定在响应中仅返回某个实体的某些属性。 您可以通过使用查询选项或使用 select 子句对 WCF 数据服务查询的结果执行投影， `$select` (在 LINQ [select](../../../csharp/language-reference/keywords/select-clause.md)查询中 Visual Basic) 中进行[选择](../../../visual-basic/language-reference/queries/select-clause.md)。 有关详细信息，请参阅 [查询数据服务](querying-the-data-service-wcf-data-services.md)。  
  
 本主题中的示例使用罗斯文示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。  
  
## <a name="example"></a>示例  

 下面的示例演示一个 LINQ 查询，该查询将 Customers 实体投影到新的 CustomerAddress 类型，该类型仅包含特定地址属性以及标识属性。 此 `CustomerAddress` 类是在客户端上定义的并且具有客户端库可将它识别为实体类型的特性。  
  
 [!code-csharp[Astoria Northwind Client#SelectCustomerAddress](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#selectcustomeraddress)]
 [!code-vb[Astoria Northwind Client#SelectCustomerAddress](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#selectcustomeraddress)]  
  
## <a name="example"></a>示例  

 下面的示例演示一个 LINQ 查询，该查询将返回的 Customers 实体投影到新的 CustomerAddressNonEntity 类型，该类型仅包含特定地址属性，不包含标识属性。 此 `CustomerAddressNonEntity` 类是在客户端上定义的，但不具有作为实体类型的特性。  
  
 [!code-csharp[Astoria Northwind Client#SelectCustomerAddressNonEntity](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#selectcustomeraddressnonentity)]
 [!code-vb[Astoria Northwind Client#SelectCustomerAddressNonEntity](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#selectcustomeraddressnonentity)]  
  
## <a name="example"></a>示例  

 下面的示例演示 `CustomerAddress` `CustomerAddressNonEntity` 前面的示例中使用的和类型的定义。  
  
 [!code-csharp[Astoria Northwind Client#CustomerAddressDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customeraddress.cs#customeraddressdefinition)]
 [!code-vb[Astoria Northwind Client#CustomerAddressDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customeraddress.vb#customeraddressdefinition)]
