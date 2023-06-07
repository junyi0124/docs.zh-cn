---
title: 推断列
ms.date: 03/30/2017
ms.assetid: 0e022699-c922-454c-93e2-957dd7e7247a
ms.openlocfilehash: 45d27b78b5d83d333c16192e172e7b7e3dd88c10
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164696"
---
# <a name="inferring-columns"></a>推断列

ADO.NET 从 XML 文档中确定了哪些元素要作为 <xref:System.Data.DataSet> 的表进行推断后，即开始推断这些表的列。 ADO.NET 2.0 引入了一个新的架构推断引擎，该引擎可推断每个 **simpleType** 元素的强类型化数据类型。 在以前的版本中，推断的 **simpleType** 元素的数据类型始终为 **xsd： string**。  
  
## <a name="migration-and-backward-compatibility"></a>迁移和向后兼容性  

 **ReadXml**方法采用**InferSchema**类型的参数。 使用此自变量可以指定与以前的版本兼容的推断行为。 下表显示了 **InferSchema** 枚举的可用值。  
  
 <xref:System.Data.XmlReadMode.InferSchema>  
 通过始终将简单类型作为 <xref:System.String> 进行推断，提供向后兼容性。  
  
 <xref:System.Data.XmlReadMode.InferTypedSchema>  
 推断强类型化的数据类型。 如果与 <xref:System.Data.DataTable> 一起使用，会发生异常。  
  
 <xref:System.Data.XmlReadMode.IgnoreSchema>  
 忽略任何内联架构并将数据读入现有的 <xref:System.Data.DataSet> 架构。  
  
## <a name="attributes"></a>特性  

 如 [推断表](inferring-tables.md)中所定义的，具有属性的元素将被推断为表。 然后，该元素的属性将被推断为该表的列。 列的 **ColumnMapping** 属性将设置为 **mappingtype.attribute**，以确保在架构写回 XML 时，列名将写入为属性。 这些属性的值存储在表的行中。 例如，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1 attr1="value1" attr2="value2"/>  
</DocumentElement>  
```  
  
 推理过程将生成一个名为 **Element1** 的表，该表包含两列： **attr1** 和 **attr2**。 这两个列的 **ColumnMapping** 属性将设置为 **mappingtype.attribute**。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|attr1|attr2|  
|-----------|-----------|  
|value1|value2|  
  
## <a name="elements-without-attributes-or-child-elements"></a>不具有属性或子元素的元素  

 如果元素不具有子元素或属性，它将被推断为列。 列的 **ColumnMapping** 属性将设置为 **mappingtype.attribute**。 子元素的文本存储在表的行中。 例如，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1>  
    <ChildElement1>Text1</ChildElement1>  
    <ChildElement2>Text2</ChildElement2>  
  </Element1>  
</DocumentElement>  
```  
  
 推理过程将生成一个名为 **Element1** 的表，该表包含两列： **ChildElement1** 和 **ChildElement2**。 这两个列的 **ColumnMapping** 属性将设置为 **mappingtype.attribute**。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|ChildElement1|ChildElement2|  
|-------------------|-------------------|  
|Text1|Text2|  
  
## <a name="see-also"></a>请参阅

- [从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)
- [从 XML 加载数据集](loading-a-dataset-from-xml.md)
- [从 XML 加载数据集架构信息](loading-dataset-schema-information-from-xml.md)
- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
