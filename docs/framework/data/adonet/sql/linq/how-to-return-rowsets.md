---
title: 如何：返回行集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 725718f5-da29-4841-9f53-aafef64ba977
ms.openlocfilehash: be03107db73ed230a87c2518e7825461afc2bc7b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155739"
---
# <a name="how-to-return-rowsets"></a>如何：返回行集

此示例从数据库中返回行集合，并包含用于筛选结果的输入参数。  
  
 当执行返回行集的存储过程时，请使用存储过程中存储返回的 *结果* 类。 有关详细信息，请参阅 [分析 LINQ to SQL 源代码](analyzing-linq-to-sql-source-code.md)。  
  
## <a name="example"></a>示例  

 下面的示例表示一个存储过程，该存储过程返回客户行并使用输入参数来仅返回将“London”列为客户城市的那些行。 该示例假定有一个可枚举的 `CustomersByCityResult` 类。  
  
```sql  
CREATE PROCEDURE [dbo].[Customers By City]  
    (@param1 NVARCHAR(20))  
AS  
BEGIN  
    -- SET NOCOUNT ON added to prevent extra result sets from  
    -- interfering with SELECT statements.  
    SET NOCOUNT ON;  
    SELECT CustomerID, ContactName, CompanyName, City from Customers  
        as c where c.City=@param1  
END  
```  
  
 [!code-csharp[DLinqSprox#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/northwind-sprox.cs#1)]
 [!code-vb[DLinqSprox#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/northwind-sprox.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [存储过程](stored-procedures.md)
- [下载示例数据库](downloading-sample-databases.md)
