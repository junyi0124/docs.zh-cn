---
title: 跨关系查询
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 297878d0-685b-4c01-b2e0-9d731b7322bc
ms.openlocfilehash: 24ab13a1d67eac39c7b3d7be8cb1c16ec7265d5e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91184872"
---
# <a name="querying-across-relationships"></a>跨关系查询

您的类定义中对其他对象或其他对象的集合的直接引用相当于数据库中的外键关系。 您可以通过使用点表示法在查询时利用这些关系来访问关系属性以及从一个对象定位到另一个对象。 这些访问操作会转换成用等效的 SQL 表示的更为复杂的联接或关联子查询。  
  
 例如，下面的查询从订单定位到客户，以此方式将结果限制为只包括位于伦敦的客户的订单。  
  
 [!code-csharp[DLinqQueryConcepts#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#3)]
 [!code-vb[DLinqQueryConcepts#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#3)]  
  
 如果关系属性不存在，则必须以 *联接*的形式手动编写它们，就像在 SQL 查询中那样，如以下代码所示：  
  
 [!code-csharp[DLinqQueryConcepts#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#4)]
 [!code-vb[DLinqQueryConcepts#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#4)]  
  
 您可以使用 " *关系* " 属性一次定义此特定关系。 然后您就可以使用更为方便的点语法。 但之所以存在关系属性，更重要的原因在于特定于域的对象模型通常定义为层次结构或关系图。 您要进行编程的那些对象引用其他对象。 如果对象与对象的关系相当于数据库中外键类型的关系，那么这只是一种令人庆幸的巧合而已。 在这种情况下，可以很方便地通过访问属性来编写联接。  
  
 就这一点而言，关系属性在查询的结果方面要比作为查询本身的一部分重要。 在查询检索到关于特定客户的数据后，类定义会指示客户下了订单。 换言之，您希望特定客户的 `Orders` 属性是用该客户下的所有订单填充的集合。 这实际上是您通过以此方式定义类声明的协定。 即便查询并未请求订单，您也希望在那里看到这些订单。 您希望自己的对象模型保持这样的错觉：它是数据库在内存中的扩展，并且相关对象立即可用。  
  
 既然您已经具备了关系，您就可以通过引用您的类中定义的关系属性来编写查询。 这些关系引用相当于数据库中的外键关系。 使用这些关系的操作会转换成用等效的 SQL 表示的更为复杂的联接。 只要你已经定义关系（使用 <xref:System.Data.Linq.Mapping.AssociationAttribute> 属性），你就无需在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中编写显式联接的代码。  
  
 为了帮助保持这种错觉， [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 实现了一种称为 *延迟加载*的技术。 有关详细信息，请参阅 [延迟与立即加载](deferred-versus-immediate-loading.md)。  
  
 请考虑以下 SQL 查询来投影对的列表 `CustomerID` - `OrderID` ：  
  
```sql
SELECT t0.CustomerID, t1.OrderID  
FROM   Customers AS t0 INNER JOIN  
          Orders AS t1 ON t0.CustomerID = t1.CustomerID  
WHERE  (t0.City = @p0)  
```  
  
 若要通过使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 获得相同的结果，您可以使用 `Orders` 类中已经存在的 `Customer` 属性引用。 `Orders`引用提供了执行查询和投影对所需的信息 `CustomerID` - `OrderID` ，如以下代码所示：  
  
 [!code-csharp[DLinqQueryConcepts#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#5)]
 [!code-vb[DLinqQueryConcepts#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#5)]  
  
 您也可以反向操作。 也就是说，您可以查询 `Orders`，然后使用其 `Customer` 关系引用来访问关于关联的 `Customer` 对象的信息。 下面的代码将与之前相同的对进行投影 `CustomerID` - `OrderID` ，但这一次是通过查询 `Orders` 而不是 `Customers` 。  
  
 [!code-csharp[DLinqQueryConcepts#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#6)]
 [!code-vb[DLinqQueryConcepts#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#6)]  
  
## <a name="see-also"></a>请参阅

- [查询概念](query-concepts.md)
