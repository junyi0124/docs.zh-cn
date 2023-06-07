---
title: 如何：将表表示为类
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 84dda12b-88a2-4cd2-92b3-8db87b28d14c
ms.openlocfilehash: c02922351909b097dc06085eefbcc0f131350505
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166217"
---
# <a name="how-to-represent-tables-as-classes"></a>如何：将表表示为类

使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Mapping.TableAttribute> 属性将类指定为与数据库表关联的实体类。  
  
### <a name="to-map-a-class-to-a-database-table"></a>将类映射到数据库表  
  
- 将 <xref:System.Data.Linq.Mapping.TableAttribute> 属性添加到类声明中。  
  
## <a name="example"></a>示例  

 下面的代码创建作为与 `Customer` 数据库表关联的实体类的 `Customers` 类。  
  
 [!code-csharp[DLinqCustomize#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCustomize/cs/Program.cs#1)]
 [!code-vb[DLinqCustomize#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCustomize/vb/Module1.vb#1)]  
  
 如果可以推断出名称，您无需指定 <xref:System.Data.Linq.Mapping.TableAttribute.Name%2A> 属性。 如果您未指定名称，则假定此名称与此属性或字段的名称相同。  
  
## <a name="see-also"></a>请参阅

- [LINQ to SQL 对象模型](the-linq-to-sql-object-model.md)
- [如何：通过使用代码编辑器自定义实体类](how-to-customize-entity-classes-by-using-the-code-editor.md)
