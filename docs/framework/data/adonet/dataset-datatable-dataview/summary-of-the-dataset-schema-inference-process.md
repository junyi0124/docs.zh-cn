---
title: 数据集架构接口过程摘要
ms.date: 03/30/2017
ms.assetid: fd0891c8-d068-4e30-a76f-7c375f078bf7
ms.openlocfilehash: 8d517487b96aa7f204ea9f25d326500db7df413a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198504"
---
# <a name="summary-of-the-dataset-schema-inference-process"></a>数据集架构接口过程摘要

推断过程首先从 XML 文档中确定将哪些元素推断为表。 从剩余的 XML 中，推断过程确定这些表的列。 对于嵌套表，推断过程会生成嵌套的 <xref:System.Data.DataRelation> 和 <xref:System.Data.ForeignKeyConstraint> 对象。  
  
 下面概述了推理规则：  
  
- 具有属性的元素会被推断为表。  
  
- 具有子元素的元素会被推断为表。  
  
- 重复的元素会被推断为单个表。  
  
- 如果文档元素（即根元素）不具有属性和将被推断为列的子元素，则将被推断为 <xref:System.Data.DataSet>。 否则，文档元素会被推断为表。  
  
- 属性会被推断为列。  
  
- 不具有属性或子元素且不重复的元素会被推断为列。  
  
- 对于在其他元素中推断为嵌套表的元素，这些元素也被推断为表，则在这两个表之间创建嵌套的 **DataRelation** 。 名为 **TableName_Id** 的新主键列将添加到这两个表中，并由 **DataRelation**使用。 使用**TableName_Id**列在两个表之间创建一个**ForeignKeyConstraint** 。  
  
- 对于推断为表并包含文本但没有子元素的元素，将为每个元素的文本创建一个名为 **TableName_Text** 的新列。 如果元素被推断为表并包含文本和子元素，则将忽略文本。  
  
## <a name="see-also"></a>请参阅

- [从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)
- [从 XML 加载数据集](loading-a-dataset-from-xml.md)
- [从 XML 加载数据集架构信息](loading-dataset-schema-information-from-xml.md)
- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
