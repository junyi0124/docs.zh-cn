---
title: 将 keyref XML 架构 (XSD) 约束映射到数据集约束
ms.date: 03/30/2017
ms.assetid: 5b634fea-cc1e-4f6b-9454-10858105b1c8
ms.openlocfilehash: fe53232b6818b5bb28b433c4a473b6381b8e9083
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198556"
---
# <a name="map-keyref-xml-schema-xsd-constraints-to-dataset-constraints"></a>将 keyref XML 架构 (XSD) 约束映射到数据集约束

**Keyref**元素使你可以在文档中的元素之间建立链接。 它类似于关系数据库中的外键关系。 如果架构指定了 **keyref** 元素，则在架构映射过程中将元素转换为的表中的列的相应外键约束 <xref:System.Data.DataSet> 。 默认情况下， **keyref** 元素还会生成一个关系，并在关系上指定 **ParentTable**、 **ChildTable**、 **ParentColumn**和 **ChildColumn** 属性。  
  
 下表概述了可在**keyref**元素中指定的**msdata**属性。  
  
|属性名称|描述|  
|--------------------|-----------------|  
|**msdata:ConstraintOnly**|如果在架构的**keyref**元素中指定了**ConstraintOnly = "true"** ，则会创建一个约束，但不会创建任何关系。 如果未指定此特性 (或设置为 **False**) ，则在 **数据集中**创建约束和关系。|  
|**msdata:ConstraintName**|如果指定了 **ConstraintName** 属性，则将其值用作约束的名称。 否则，架构中**keyref**元素的**Name**属性提供**数据集中**的约束名称。|  
|**msdata:UpdateRule**|如果在架构的**keyref**元素中指定了**UpdateRule**属性，则其值将分配给**数据集中**的**UpdateRule**约束属性。 否则， **UpdateRule** 属性将设置为 **Cascade**。|  
|**msdata:DeleteRule**|如果在架构的**keyref**元素中指定了**DeleteRule**属性，则其值将分配给**数据集中**的**DeleteRule**约束属性。 否则， **DeleteRule** 属性将设置为 **Cascade**。|  
|**msdata:AcceptRejectRule**|如果在架构的**keyref**元素中指定了**AcceptRejectRule**属性，则其值将分配给**数据集中**的**AcceptRejectRule**约束属性。 否则， **AcceptRejectRule** 属性设置为 **None**。|  
  
 下面的示例包含一个架构，该架构指定**Order**元素的**OrderNumber**子元素和**OrderDetail**元素的**OrderNo**子元素之间的**key**和**keyref**关系。  
  
 在此示例中， **OrderDetail**元素的**OrderNumber**子元素引用**Order**元素的**OrderNo** key 子元素。  
  
```xml  
<xs:schema id="MyDataSet" xmlns=""
            xmlns:xs="http://www.w3.org/2001/XMLSchema"
            xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  
 <xs:element name="MyDataSet" msdata:IsDataSet="true">  
  <xs:complexType>  
    <xs:choice maxOccurs="unbounded">  
      <xs:element name="OrderDetail">  
       <xs:complexType>  
         <xs:sequence>  
           <xs:element name="OrderNo" type="xs:integer" />  
           <xs:element name="ItemNo" type="xs:string" />  
         </xs:sequence>  
       </xs:complexType>  
      </xs:element>  
      <xs:element name="Order">  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="OrderNumber" type="xs:integer" />  
            <xs:element name="EmpNumber" type="xs:integer" />  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:choice>  
  </xs:complexType>  
  
  <xs:key name="OrderNumberKey"  >  
    <xs:selector xpath=".//Order" />  
    <xs:field xpath="OrderNumber" />  
  </xs:key>  
  
  <xs:keyref name="OrderNoRef" refer="OrderNumberKey">  
    <xs:selector xpath=".//OrderDetail" />  
    <xs:field xpath="OrderNo" />  
  </xs:keyref>  
 </xs:element>  
</xs:schema>  
```  
  
 XML 架构定义语言 (XSD) 架构映射过程生成包含两个表的以下 **数据集** ：  
  
```text  
OrderDetail(OrderNo, ItemNo) and  
Order(OrderNumber, EmpNumber)  
```  
  
 此外， **数据集** 还定义以下约束：  
  
- **Order**表的唯一约束。  
  
    ```text
              Table: Order  
    Columns: OrderNumber
    ConstraintName: OrderNumberKey  
    Type: UniqueConstraint  
    IsPrimaryKey: False  
    ```  
  
- **Order**表和**OrderDetail**表之间的关系。 **嵌套**的属性设置为**False** ，因为这两个元素不嵌套在架构中。  
  
    ```text
              ParentTable: Order  
    ParentColumns: OrderNumber
    ChildTable: OrderDetail  
    ChildColumns: OrderNo
    ParentKeyConstraint: OrderNumberKey  
    ChildKeyConstraint: OrderNoRef  
    RelationName: OrderNoRef  
    Nested: False  
    ```  
  
- **OrderDetail**表的外键约束。  
  
    ```text  
              ConstraintName: OrderNoRef  
    Type: ForeignKeyConstraint  
    Table: OrderDetail  
    Columns: OrderNo
    RelatedTable: Order  
    RelatedColumns: OrderNumber
    ```  
  
## <a name="see-also"></a>请参阅

- [将关键 XML 架构 (XSD) 约束映射到数据集约束](mapping-xml-schema-xsd-constraints-to-dataset-constraints.md)
- [从 XML 架构生成数据集关系 (XSD)](generating-dataset-relations-from-xml-schema-xsd.md)
- [ADO.NET 概述](../ado-net-overview.md)
