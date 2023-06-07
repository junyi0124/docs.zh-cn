---
title: 如何：将列表示为时间戳列或版本列
ms.date: 03/30/2017
ms.assetid: 5afd5ce8-1d20-4bc3-a34f-49d95449f493
ms.openlocfilehash: cc8538ab7b2ecf5183cfb97995c04648493a369f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191744"
---
# <a name="how-to-represent-columns-as-timestamp-or-version-columns"></a>如何：将列表示为时间戳列或版本列

使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A> 特性的属性 <xref:System.Data.Linq.Mapping.ColumnAttribute> 可将字段或属性指定为表示保存数据库时间戳或版本号的数据库列。  
  
 有关代码示例，请参见<xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A>。  
  
### <a name="to-designate-a-field-or-property-as-representing-a-timestamp-or-version-column"></a>将字段或属性指定为表示时间戳列或版本列  
  
1. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A> 属性 (Property) 添加到 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute)。  
  
2. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A> 属性值设置为 `true`。  
  
## <a name="see-also"></a>请参阅

- [LINQ to SQL 对象模型](the-linq-to-sql-object-model.md)
- [如何：指定要对哪些成员进行并发冲突测试](how-to-specify-which-members-are-tested-for-concurrency-conflicts.md)
- [如何：通过使用代码编辑器自定义实体类](how-to-customize-entity-classes-by-using-the-code-editor.md)
