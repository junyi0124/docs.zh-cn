---
title: 从 XML 架构生成数据集关系 (XSD)
ms.date: 03/30/2017
ms.assetid: 1c9a1413-c0d2-4447-88ba-9a2b0cbc0aa8
ms.openlocfilehash: 2673280ebb94dcc10c130f3969f3e3250d3706a2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198582"
---
# <a name="generating-dataset-relations-from-xml-schema-xsd"></a>从 XML 架构生成数据集关系 (XSD)

在 <xref:System.Data.DataSet> 中，可通过创建父子关系来形成两个或更多个列之间的关联。 有三种方法可以表示 XML 架构定义语言中的 **数据集** 关系 (XSD) 架构：  
  
- 指定嵌套复杂类型。  
  
- 使用 **msdata： Relationship** 批注。  
  
- 指定不带**msdata： ConstraintOnly**批注的**xs： keyref** 。  
  
## <a name="nested-complex-types"></a>嵌套的复杂类型  

 架构中的嵌套复杂类型定义指示元素的父子关系。 下面的 XML 架构片段显示， **OrderDetail** 是 **Order** 元素的子元素。  
  
```xml  
<xs:element name="Order">  
  <xs:complexType>  
     <xs:sequence>
       <xs:element name="OrderDetail" />  
           <xs:complexType>
           </xs:complexType>  
     </xs:sequence>  
  </xs:complexType>  
</xs:element>  
```  
  
 XML 架构映射过程在 **数据集中** 创建与架构中的嵌套复杂类型相对应的表。 它还将创建用作 **-** 生成的表的父子列的其他列。 请注意，这些父子 **-** 列指定关系，这不同于指定主键/外键约束。  
  
## <a name="msdatarelationship-annotation"></a>msdata:Relationship 批注  

 **Msdata： Relationship**批注允许您显式指定架构中未嵌套的元素之间的父子关系。 下面的示例显示了 **关系** 元素的结构。  
  
```xml  
<msdata:Relationship name="CustOrderRelationship"
msdata:parent=""
msdata:child=""
msdata:parentkey=""
msdata:childkey="" />  
```  
  
 **Msdata： Relationship**批注的属性标识父子关系中涉及的元素，以及关系中涉及的**parentkey**和**childkey**元素和属性。 映射过程使用此信息在 **数据集中** 生成表，并在这些表之间创建主键/外键关系。  
  
 例如，下面的架构片段指定 (未嵌套) 的同一级别的 **Order** 和 **OrderDetail** 元素。 该架构指定 **msdata： Relationship** 批注，该批注指定这两个元素之间的父子关系。 在这种情况下，必须使用 **msdata： relationship** 批注指定显式关系。  
  
```xml  
 <xs:element name="MyDataSet" msdata:IsDataSet="true">  
  <xs:complexType>  
    <xs:choice maxOccurs="unbounded">  
        <xs:element name="OrderDetail">  
          <xs:complexType>  
  
          </xs:complexType>  
       </xs:element>  
       <xs:element name="Order">  
          <xs:complexType>  
  
          </xs:complexType>  
       </xs:element>  
    </xs:choice>  
  </xs:complexType>  
</xs:element>  
   <xs:annotation>  
     <xs:appinfo>  
       <msdata:Relationship name="OrdOrdDetailRelation"  
          msdata:parent="Order"  
          msdata:child="OrderDetail"
          msdata:parentkey="OrderNumber"  
          msdata:childkey="OrderNo"/>  
     </xs:appinfo>  
  </xs:annotation>  
```  
  
 映射过程使用**Relationship**元素在**Order**表的**OrderNumber**列和**数据集**的**OrderDetail**表中的**OrderNo**列之间创建父子关系。 映射进程仅指定关系；与关系数据库中的主键/外键约束不同，它并不会自动为这些列中的值指定任何约束。  
  
### <a name="in-this-section"></a>本节内容  

 [映射嵌套架构元素之间的隐式关系](map-implicit-relations-between-nested-schema-elements.md)  
 描述在 XML 架构中遇到嵌套元素时在 **数据集中** 隐式创建的约束和关系。  
  
 [映射为嵌套元素指定的关系](map-relations-specified-for-nested-elements.md)  
 介绍如何在 **数据集中** 为 XML 架构中的嵌套元素显式设置关系。  
  
 [指定无嵌套的元素之间的关系](specify-relations-between-elements-with-no-nesting.md)  
 介绍如何在未嵌套的 XML 架构元素之间创建 **数据集中** 的关系。  
  
### <a name="related-sections"></a>相关章节  

 [从 XML 架构派生数据集关系结构 (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)  
 描述从 XML 架构定义语言 (XSD) 架构创建的 **数据集** 的关系结构（或架构）。  
  
 [将关键 XML 架构 (XSD) 约束映射到数据集约束](mapping-xml-schema-xsd-constraints-to-dataset-constraints.md)  
 介绍用于在 **数据集中**创建唯一约束和外键约束的 XML 架构元素。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
