---
title: XML 架构约束和关系
ms.date: 03/30/2017
ms.assetid: 165bc2bc-60a1-40e0-9b89-7c68ef979079
ms.openlocfilehash: 5861386e42defa189aaa50a3af0bd95d7e9257fd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173700"
---
# <a name="xml-schema-constraints-and-relationships"></a>XML 架构约束和关系

在 (XSD) 架构的 XML 架构定义语言中，可以使用 **msdata： Relationship** 批注 (指定 (unique、key 和 keyref 约束) 和关系的约束。 本主题说明如何解释在 XML 架构中指定的约束和关系，以生成 <xref:System.Data.DataSet>。  
  
 通常，在 XML 架构中，如果只想在**DataSet**中生成关系，则可指定**msdata： Relationship**批注。 有关详细信息，请参阅 [从 XML 架构生成数据集关系 (XSD) ](generating-dataset-relations-from-xml-schema-xsd.md)。 如果要在 **DataSet**中生成约束，请 (unique、key 和 keyref) 指定约束。 请注意，如本主题稍后所述，键和 keyref 约束也可用于生成关系。  
  
## <a name="generating-a-relationship-from-key-and-keyref-constraints"></a>从键和 keyref 约束生成关系  

 您可以不指定 **msdata： Relationship** 批注，而是指定在 XML 架构映射过程中使用的键和 keyref 约束，以便不仅生成约束，还生成 **数据集中**的关系。 但是，如果 `msdata:ConstraintOnly="true"` 在 **keyref** 元素中指定，则 **数据集会** 只包含约束，而不包括关系。  
  
 下面的示例显示一个 XML 架构，该架构包含不嵌套的 **Order** 和 **OrderDetail** 元素。 该架构还指定键约束和 keyref 约束。  
  
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
  
 在 XML 架构映射过程中生成的 **数据集** 包含 **Order** 和 **OrderDetail** 表。 此外， **数据集** 还包括关系和约束。 以下示例将显示这些关系和约束。 请注意，架构未指定 **msdata： Relationship** 批注;相反，键和 keyref 约束用于生成关系。  
  
```text
....ConstraintName: OrderNumberKey  
....Type: UniqueConstraint  
....Table: Order  
....Columns: OrderNumber  
....IsPrimaryKey: False  
  
....ConstraintName: OrderNoRef  
....Type: ForeignKeyConstraint  
....Table: OrderDetail  
....Columns: OrderNo  
....RelatedTable: Order  
....RelatedColumns: OrderNumber  
  
..RelationName: OrderNoRef  
..ParentTable: Order  
..ParentColumns: OrderNumber  
..ChildTable: OrderDetail  
..ChildColumns: OrderNo  
..ParentKeyConstraint: OrderNumberKey  
..ChildKeyConstraint: OrderNoRef  
..Nested: False  
```  
  
 在上面的架构示例中， **Order** 和 **OrderDetail** 元素不嵌套。 在以下架构示例中，这些元素嵌套。 但未指定 **msdata： Relationship** 批注;因此，假定使用隐式关系。 有关详细信息，请参阅 [映射嵌套架构元素之间的隐式关系](map-implicit-relations-between-nested-schema-elements.md)。 该架构还指定键约束和 keyref 约束。  
  
```xml  
<xs:schema id="MyDataSet" xmlns=""
            xmlns:xs="http://www.w3.org/2001/XMLSchema"
            xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  
 <xs:element name="MyDataSet" msdata:IsDataSet="true">  
  <xs:complexType>  
    <xs:choice maxOccurs="unbounded">  
  
      <xs:element name="Order">  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="OrderNumber" type="xs:integer" />  
            <xs:element name="EmpNumber" type="xs:integer" />  
  
            <xs:element name="OrderDetail">  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="OrderNo" type="xs:integer" />  
                  <xs:element name="ItemNo" type="xs:string" />  
                </xs:sequence>  
              </xs:complexType>  
            </xs:element>  
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
  
 XML 架构映射过程中生成的 **数据集** 包括两个表：  
  
```text  
Order(OrderNumber, EmpNumber, Order_Id)  
OrderDetail(OrderNumber, ItemNumber, Order_Id)  
```  
  
 **数据集**还包括两个关系 (一个基于**msdata： relationship**批注，另一个基于键和 keyref 约束) 和各种约束。 以下示例将显示这些关系和约束。  
  
```text
..RelationName: Order_OrderDetail  
..ParentTable: Order  
..ParentColumns: Order_Id  
..ChildTable: OrderDetail  
..ChildColumns: Order_Id  
..ParentKeyConstraint: Constraint1  
..ChildKeyConstraint: Order_OrderDetail  
..Nested: True  
  
..RelationName: OrderNoRef  
..ParentTable: Order  
..ParentColumns: OrderNumber  
..ChildTable: OrderDetail  
..ChildColumns: OrderNo  
..ParentKeyConstraint: OrderNumberKey  
..ChildKeyConstraint: OrderNoRef  
..Nested: False  
  
..ConstraintName: OrderNumberKey  
..Type: UniqueConstraint  
..Table: Order  
..Columns: OrderNumber  
..IsPrimaryKey: False  
  
..ConstraintName: Constraint1  
..Type: UniqueConstraint  
..Table: Order  
..Columns: Order_Id  
..IsPrimaryKey: True  
  
..ConstraintName: Order_OrderDetail  
..Type: ForeignKeyConstraint  
..Table: OrderDetail  
..Columns: Order_Id  
..RelatedTable: Order  
..RelatedColumns: Order_Id  
  
..ConstraintName: OrderNoRef  
..Type: ForeignKeyConstraint  
..Table: OrderDetail  
..Columns: OrderNo  
..RelatedTable: Order  
..RelatedColumns: OrderNumber  
```  
  
 如果引用嵌套表的 keyref 约束包含 **msdata： IsNested = "true"** 批注，则该 **数据集** 将创建基于 keyref 约束的单个嵌套关系以及相关的 unique/key 约束。  
  
## <a name="see-also"></a>请参阅

- [从 XML 架构派生数据集关系结构 (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)
- [ADO.NET 概述](../ado-net-overview.md)
