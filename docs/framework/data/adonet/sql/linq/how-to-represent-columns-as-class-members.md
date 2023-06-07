---
title: 如何：将列表示为类成员
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7ab28021-4d15-4d9c-bf2e-6ccc0daa7d1a
ms.openlocfilehash: e73b017b5a500a8c48b3fe22557f6c6619f3b227
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166347"
---
# <a name="how-to-represent-columns-as-class-members"></a>如何：将列表示为类成员

使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性可将字段或属性与数据库列关联。  
  
### <a name="to-map-a-field-or-property-to-a-database-column"></a>将字段或属性映射到数据库列  
  
- 将 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute) 添加到相应属性 (Property) 或字段的声明中。  
  
## <a name="example"></a>示例  

 下面的代码将 `CustomerID` 类中的 `Customer` 字段映射到 `CustomerID` 数据库表中的 `Customers` 列。  
  
 [!code-csharp[DLinqCustomize#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCustomize/cs/Program.cs#2)]
 [!code-vb[DLinqCustomize#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCustomize/vb/Module1.vb#2)]  
  
 如果可以推断出名称，您无需指定 <xref:System.Data.Linq.Mapping.DataAttribute.Name%2A> 属性。 如果您未指定名称，则假定此名称与此属性或字段的名称相同。  
  
## <a name="see-also"></a>请参阅

- [LINQ to SQL 对象模型](the-linq-to-sql-object-model.md)
- [如何：通过使用代码编辑器自定义实体类](how-to-customize-entity-classes-by-using-the-code-editor.md)
