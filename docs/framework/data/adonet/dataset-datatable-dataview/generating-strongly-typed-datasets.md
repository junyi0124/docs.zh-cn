---
title: 生成强类型化数据集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 54333cbf-bb43-4314-a7d4-6dc1dd1c44b3
ms.openlocfilehash: 1c65389c8c5664f86f3f0c04829a2422908d72d1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202287"
---
# <a name="generating-strongly-typed-datasets"></a>生成强类型化数据集

如果给定的 XML 架构符合 XML 架构定义语言 (XSD) standard，则可以 <xref:System.Data.DataSet> 使用随 Windows 软件开发工具包 (SDK) 提供的 XSD.exe 工具生成强类型化。  
  
  (从数据库表创建 xsd，请参阅 <xref:System.Data.DataSet.WriteXmlSchema%2A> 或使用 [Visual Studio 中的数据集](/visualstudio/data-tools/dataset-tools-in-visual-studio)) 。  
  
 下面的代码演示了使用此工具生成 **数据集** 的语法。  
  
```console  
xsd.exe /d /l:CS XSDSchemaFileName.xsd /eld /n:XSDSchema.Namespace  
```  
  
 在此语法中， `/d` 指令告诉工具生成 **数据集**，并 `/l:` 告诉工具使用哪种语言 (例如，c # 或 Visual Basic .net) 。 可选 `/eld` 指令指定可以使用 LINQ to DataSet 来针对生成的**数据集**进行查询。 当同时指定 `/d` 选项时可使用此选项。 有关详细信息，请参阅 [查询类型化数据集](../querying-typed-datasets.md)。 可选 `/n:` 指令指示该工具还会生成一个名为**XSDSchema**的**数据集**的命名空间。 命令的输出为 XSDSchemaFileName.cs，该输出可以在 ADO.NET 应用程序中编译和使用。 所生成的代码可以编译成库或模块。  
  
 以下代码显示使用 C# 编译器 (csc.exe) 将生成的代码编译成库的语法。  
  
```console  
csc.exe /t:library XSDSchemaFileName.cs /r:System.dll /r:System.Data.dll  
```  
  
 `/t:` 指令指示该工具编译成库，`/r:` 指令指定进行编译所需的依赖库。 该命令的输出为 XSDSchemaFileName.dll，它可以在使用 `/r:` 指令编译 ADO.NET 应用程序时传递给编译器。  
  
 以下代码显示访问向 ADO.NET 应用程序中的 XSD.exe 传递的命名空间的语法。  
  
```vb  
Imports XSDSchema.Namespace  
```  
  
```csharp  
using XSDSchema.Namespace;  
```  
  
 下面的代码示例使用名为**CustomerDataSet**的类型化**数据集**从**Northwind**数据库加载客户列表。 使用**Fill**方法加载数据后，该示例会使用类型化的**CustomersRow** (**DataRow**) 对象循环通过**Customers**表中的每个客户。 这提供了对 **CustomerID** 列的直接访问，而不是通过 **DataColumnCollection**。  
  
```vb  
Dim customers As CustomerDataSet= New CustomerDataSet()  
Dim adapter As SqlDataAdapter New SqlDataAdapter( _  
  "SELECT * FROM dbo.Customers;", _  
  "Data Source=(local);Integrated " & _  
  "Security=SSPI;Initial Catalog=Northwind")  
  
adapter.Fill(customers, "Customers")  
  
Dim customerRow As CustomerDataSet.CustomersRow  
For Each customerRow In customers.Customers  
  Console.WriteLine(customerRow.CustomerID)  
Next  
```  
  
```csharp  
CustomerDataSet customers = new CustomerDataSet();  
SqlDataAdapter adapter = new SqlDataAdapter(  
  "SELECT * FROM dbo.Customers;",  
  "Data Source=(local);Integrated " +  
  "Security=SSPI;Initial Catalog=Northwind");  
  
adapter.Fill(customers, "Customers");  
  
foreach(CustomerDataSet.CustomersRow customerRow in customers.Customers)  
  Console.WriteLine(customerRow.CustomerID);  
```  
  
 下面是用于示例的 XML 架构：
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema id="CustomerDataSet" xmlns="" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  <xs:element name="CustomerDataSet" msdata:IsDataSet="true">  
    <xs:complexType>  
      <xs:choice maxOccurs="unbounded">  
        <xs:element name="Customers">  
          <xs:complexType>  
            <xs:sequence>  
              <xs:element name="CustomerID" type="xs:string" minOccurs="0" />  
            </xs:sequence>  
          </xs:complexType>  
        </xs:element>  
      </xs:choice>  
    </xs:complexType>  
  </xs:element>  
</xs:schema>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataColumnCollection>
- <xref:System.Data.DataSet>
- [类型化数据集](typed-datasets.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
