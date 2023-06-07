---
title: 如何：执行返回 StructuralType 结果的查询
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2314f2a2-b1c3-40c4-95bb-cdf9b21a7b53
ms.openlocfilehash: 4b3a63cb23851c6545f330ebd2371e1a34ff13e8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198309"
---
# <a name="how-to-execute-a-query-that-returns-structuraltype-results"></a>如何：执行返回 StructuralType 结果的查询

本主题演示如何使用 <xref:System.Data.EntityClient.EntityCommand> 对象针对概念模型执行命令，以及如何使用 <xref:System.Data.Metadata.Edm.StructuralType> 检索 <xref:System.Data.EntityClient.EntityDataReader> 结果。 <xref:System.Data.Metadata.Edm.EntityType>、<xref:System.Data.Metadata.Edm.RowType> 和 <xref:System.Data.Metadata.Edm.ComplexType> 类派生自 <xref:System.Data.Metadata.Edm.StructuralType> 类。  
  
### <a name="to-run-the-code-in-this-example"></a>运行本示例中的代码  
  
1. 将 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) 添加到项目中，并将项目配置为使用实体框架。 有关详细信息，请参阅 [如何：使用实体数据模型向导](/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))。  
  
2. 在应用程序的代码页中，添加以下 `using` 语句（在 Visual Basic 中为 `Imports`）：  
  
     [!code-csharp[DP EntityServices Concepts#Namespaces](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/source.cs#namespaces)]
     [!code-vb[DP EntityServices Concepts#Namespaces](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp entityservices concepts/vb/source.vb#namespaces)]  
  
## <a name="example"></a>示例  

 本示例执行返回 <xref:System.Data.Metadata.Edm.EntityType> 结果的查询。 如果将以下查询作为自变量传递给 `ExecuteStructuralTypeQuery` 函数，则该函数将显示有关 `Products` 的详细信息：  
  
 [!code-csharp[DP EntityServices Concepts 2#SelectProduct](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#selectproduct)]  
  
 如果传递参数化查询（如下面的查询），请将 <xref:System.Data.EntityClient.EntityParameter> 对象添加到 <xref:System.Data.EntityClient.EntityCommand.Parameters%2A> 对象的 <xref:System.Data.EntityClient.EntityCommand> 属性。  
  
 [!code-csharp[DP EntityServices Concepts 2#GREATER_OR_EQUALS](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#greater_or_equals)]  
  
 [!code-csharp[DP EntityServices Concepts#eSQLStructuralTypes](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/source.cs#esqlstructuraltypes)]
 [!code-vb[DP EntityServices Concepts#eSQLStructuralTypes](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp entityservices concepts/vb/source.vb#esqlstructuraltypes)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](./language-reference/entity-sql-reference.md)
- [用于实体框架的 EntityClient 提供程序](entityclient-provider-for-the-entity-framework.md)
