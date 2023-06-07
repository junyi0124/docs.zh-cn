---
title: 将关键 XML 架构 (XSD) 约束映射到数据集约束
ms.date: 03/30/2017
ms.assetid: 22664196-f270-4ebc-a169-70e16a83dfa1
ms.openlocfilehash: b55b232faa01bf36788276caaf8bc2e97dddf697
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172783"
---
# <a name="map-key-xml-schema-xsd-constraints-to-dataset-constraints"></a>将关键 XML 架构 (XSD) 约束映射到数据集约束

在架构中，可以使用 **key** 元素对元素或属性指定键约束。 对其指定键约束的元素或属性必须在任何架构实例中都具有唯一值，并且不能具有空值。  
  
 除了对其定义键约束的列不能具有空值之外，键约束与唯一约束类似。  
  
 下表概述了可以在**key**元素中指定的**msdata**属性。  
  
|属性名称|描述|  
|--------------------|-----------------|  
|**msdata:ConstraintName**|如果指定了该属性，它的值将用作约束名。 否则， **name** 属性提供约束名称的值。|  
|**msdata:PrimaryKey**|如果 `PrimaryKey="true"` 存在，则 **IsPrimaryKey** 约束属性设置为 **true**，从而使其成为主键。 **AllowDBNull**列属性设置为**false**，因为主键不能具有 null 值。|  
  
 在转换指定了键约束的架构时，映射过程会对约束中的每一列将 **AllowDBNull** column 属性设置为 **false** 的表创建 unique 约束。 除非**IsPrimaryKey**已对 key 元素指定，否则 unique 约束的 IsPrimaryKey 属性也设置为**false** `msdata:PrimaryKey="true"` 。 **key** 它与 `PrimaryKey="true"` 的架构中的唯一约束相同。  
  
 在下面的架构示例中， **key** 元素指定了 **CustomerID** 元素的键约束。  
  
```xml  
<xs:schema id="cod"  
            xmlns:xs="http://www.w3.org/2001/XMLSchema"
            xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  <xs:element name="Customers">  
    <xs:complexType>  
      <xs:sequence>  
        <xs:element name="CustomerID" type="xs:string" minOccurs="0" />  
        <xs:element name="CompanyName" type="xs:string" minOccurs="0" />  
       <xs:element name="Phone" type="xs:string" />  
     </xs:sequence>  
   </xs:complexType>  
 </xs:element>  
<xs:element name="MyDataSet" msdata:IsDataSet="true">  
  <xs:complexType>  
    <xs:choice maxOccurs="unbounded">  
      <xs:element ref="Customers" />  
    </xs:choice>  
  </xs:complexType>  
   <xs:key  msdata:PrimaryKey="true"  
       msdata:ConstraintName="KeyCustID"  
          name="KeyConstCustomerID" >  
     <xs:selector xpath=".//Customers" />  
     <xs:field xpath="CustomerID" />  
    </xs:key>  
 </xs:element>  
</xs:schema>
```  
  
 **Key**元素指定**Customers**元素的**CustomerID**子元素的值必须具有唯一值，且不能具有 null 值。 在转换 XML 架构定义语言 (XSD) 架构时，映射过程将创建下表：  
  
```text  
Customers(CustomerID, CompanyName, Phone)  
```  
  
 XML 架构映射还会在**CustomerID**列上创建**UniqueConstraint** ，如下所示 <xref:System.Data.DataSet> 。 （为简便起见，只显示相关属性。）  
  
```text  
      DataSetName: MyDataSet  
TableName: customers  
  ColumnName: CustomerID  
      AllowDBNull: False  
      Unique: True  
  ConstraintName: KeyCustID  
      Table: customers  
      Columns: CustomerID
      IsPrimaryKey: True  
```  
  
 在生成的**数据集中**， **UniqueConstraint**的**IsPrimaryKey**属性设置为**true** ，因为架构指定 `msdata:PrimaryKey="true"` 了**key**元素中的。  
  
 **DataSet**中**UniqueConstraint**的**ConstraintName**属性的值是在架构的**Key**元素中指定的**msdata： ConstraintName**属性的值。  
  
## <a name="see-also"></a>请参阅

- [将关键 XML 架构 (XSD) 约束映射到数据集约束](mapping-xml-schema-xsd-constraints-to-dataset-constraints.md)
- [从 XML 架构生成数据集关系 (XSD)](generating-dataset-relations-from-xml-schema-xsd.md)
- [ADO.NET 概述](../ado-net-overview.md)
