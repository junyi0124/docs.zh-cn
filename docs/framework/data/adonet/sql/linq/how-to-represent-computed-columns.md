---
title: 如何：表示计算所得的列
ms.date: 03/30/2017
ms.assetid: 4025f1fd-9dfa-46c0-b04f-34e8bc7957a2
ms.openlocfilehash: d952a6c22cd96bbc89aeebfa4b13e9727a363c73
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166319"
---
# <a name="how-to-represent-computed-columns"></a>如何：表示计算所得的列

使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A> 特性上的属性 <xref:System.Data.Linq.Mapping.ColumnAttribute> 表示其内容为计算结果的列。  
  
 有关代码示例，请参见<xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A>。  
  
> [!NOTE]
> [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持作为主键的计算所得列。  
  
### <a name="to-represent-a-computed-column"></a>表示计算所得的列  
  
1. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A> 属性 (Property) 添加到 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute)。  
  
2. 将公式的字符串表示形式赋给 <xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A> 属性。  
  
## <a name="see-also"></a>请参阅

- [LINQ to SQL 对象模型](the-linq-to-sql-object-model.md)
- [如何：通过使用代码编辑器自定义实体类](how-to-customize-entity-classes-by-using-the-code-editor.md)
