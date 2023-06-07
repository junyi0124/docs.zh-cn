---
title: 为类型化的数据集进行批注
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f82aaa62-321e-4c8a-b51b-9d1114700170
ms.openlocfilehash: 79d3913827d5df6f0ac4e77bfdb8f37b553a86d2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203743"
---
# <a name="annotating-typed-datasets"></a>为类型化的数据集进行批注

批注使您能够在不修改基础架构的情况下修改类型化 <xref:System.Data.DataSet> 中元素的名称。 修改基础架构中元素的名称将导致类型化 **数据集** 引用数据源中不存在的对象，并丢失对数据源中存在的对象的引用。  
  
 使用批注，你可以使用更有意义的名称来自定义类型化 **数据集中** 的对象的名称，从而使代码更易于阅读，并且你的类型化 **数据集** 更易于供客户端使用，同时保持基础架构不变。 例如， **Northwind**数据库的**Customers**表的以下 Schema 元素会导致**DataRow**对象名称**CustomersRow**和 <xref:System.Data.DataRowCollection> 命名**客户**。  
  
```xml  
<xs:element name="Customers">  
  <xs:complexType>  
    <xs:sequence>  
      <xs:element name="CustomerID" type="xs:string" minOccurs="0" />  
    </xs:sequence>  
  </xs:complexType>  
</xs:element>  
```  
  
 客户端代码中**客户**端代码的**DataRowCollection**名称是有意义的，但**DataRow**名称**CustomersRow**会产生误导，因为它是单个对象。 此外，在常见情况下，对象会被称为没有 **行** 标识符，而只是将其称为 **Customer** 对象。 解决方法是为该架构添加注释并标识 **DataRow** 和 **DataRowCollection** 对象的新名称。 下面是上一架构的批注版本。  
  
```xml  
<xs:element name="Customers" codegen:typedName="Customer" codegen:typedPlural="Customers">  
  <xs:complexType>  
    <xs:sequence>  
      <xs:element name="CustomerID" type="xs:string" minOccurs="0" />  
    </xs:sequence>  
  </xs:complexType>  
</xs:element>  
```  
  
 将 **typedName** 值指定为 **Customer** 将导致 **DataRow** 对象名称为 **customer**。 指定**客户**的**typedPlural**值将保留**客户**的**DataRowCollection**名称。  
  
 下表显示可用的批注。  
  
|Annotation|描述|  
|----------------|-----------------|  
|**typedName**|对象的名称。|  
|**typedPlural**|对象集合的名称。|  
|**typedParent**|对象在父关系中被引用时的名称。|  
|**typedChildren**|用于从子关系中返回对象的方法的名称。|  
|**nullValue**|如果基础值为 **DBNull**，则值为。 请参阅下表了解 **nullValue** 批注。 默认值为 **_throw**。|  
  
 下表显示了可为 **nullValue** 批注指定的值。  
  
|nullValue 值|描述|  
|---------------------|-----------------|  
|*替换值*|指定要返回的值。 所返回的值必须匹配该元素的类型。 例如，使用 `nullValue="0"` 可为空整数字段返回 0。|  
|**_throw**|引发异常。 这是默认设置。|  
|**_null**|如果遇到基元类型，则返回空引用或引发异常。|  
|**_empty**|对于字符串，返回 **String。** 如果为空，则返回从空构造函数创建的对象。 如果遇到基元类型，则引发异常。|  
  
 下表显示了类型化 **数据集中** 的对象的默认值和可用的批注。  
  
|对象/方法/事件|默认|Annotation|  
|---------------------------|-------------|----------------|  
|**DataTable**|TableNameDataTable|typedPlural|  
|**DataTable** 方法|NewTableNameRow<br /><br /> AddTableNameRow<br /><br /> DeleteTableNameRow|typedName|  
|**DataRowCollection**|TableName|typedPlural|  
|**DataRow**|TableNameRow|typedName|  
|**DataColumn**|DataTable.ColumnNameColumn<br /><br /> DataRow.ColumnName|typedName|  
|**属性**|PropertyName|typedName|  
|**子** 者|GetChildTableNameRows|typedChildren|  
|**父项** 者|TableNameRow|typedParent|  
|**数据集** 事件|TableNameRowChangeEvent<br /><br /> TableNameRowChangeEventHandler|typedName|  
  
 若要使用类型化 **数据集** 批注，您必须在 XML 架构定义语言中包含以下 (XSD) 架构的 **xmlns** 引用。 若要从数据库表创建 xsd，请参阅 <xref:System.Data.DataSet.WriteXmlSchema%2A> 或 [使用 Visual Studio 中的数据集](/visualstudio/data-tools/dataset-tools-in-visual-studio)。  
  
```xml  
xmlns:codegen="urn:schemas-microsoft-com:xml-msprop"  
```  
  
 下面是一个示例批注的架构，它公开**Northwind**数据库的**Customers**表，其中包含与**Orders**表的关系。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema id="CustomerDataSet"
      xmlns:codegen="urn:schemas-microsoft-com:xml-msprop"  
      xmlns=""
      xmlns:xs="http://www.w3.org/2001/XMLSchema"
      xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  <xs:element name="CustomerDataSet" msdata:IsDataSet="true">  
    <xs:complexType>  
      <xs:choice maxOccurs="unbounded">  
        <xs:element name="Customers" codegen:typedName="Customer" codegen:typedPlural="Customers">  
          <xs:complexType>  
            <xs:sequence>  
              <xs:element name="CustomerID"  
codegen:typedName="CustomerID" type="xs:string" minOccurs="0" />  
              <xs:element name="CompanyName"  
codegen:typedName="CompanyName" type="xs:string" minOccurs="0" />  
              <xs:element name="Phone" codegen:typedName="Phone" codegen:nullValue="" type="xs:string" minOccurs="0" />  
            </xs:sequence>  
          </xs:complexType>  
        </xs:element>  
        <xs:element name="Orders" codegen:typedName="Order" codegen:typedPlural="Orders">  
          <xs:complexType>  
            <xs:sequence>  
              <xs:element name="OrderID" codegen:typedName="OrderID"  
type="xs:int" minOccurs="0" />  
              <xs:element name="CustomerID"  
codegen:typedName="CustomerID"                  codegen:nullValue="" type="xs:string" minOccurs="0" />  
              <xs:element name="EmployeeID"  
codegen:typedName="EmployeeID" codegen:nullValue="0"
type="xs:int" minOccurs="0" />  
              <xs:element name="OrderAdapter"  
codegen:typedName="OrderAdapter" codegen:nullValue="1980-01-01T00:00:00"
type="xs:dateTime" minOccurs="0" />  
            </xs:sequence>  
          </xs:complexType>  
        </xs:element>  
      </xs:choice>  
    </xs:complexType>  
    <xs:unique name="Constraint1">  
      <xs:selector xpath=".//Customers" />  
      <xs:field xpath="CustomerID" />  
    </xs:unique>  
    <xs:keyref name="CustOrders" refer="Constraint1"  
codegen:typedParent="Customer" codegen:typedChildren="GetOrders">  
      <xs:selector xpath=".//Orders" />  
      <xs:field xpath="CustomerID" />  
    </xs:keyref>  
  </xs:element>  
</xs:schema>  
```  
  
 下面的代码示例使用从示例架构创建的强类型化 **数据集** 。 它使用一个 <xref:System.Data.SqlClient.SqlDataAdapter> 填充 **Customers** 表，另一个用于 <xref:System.Data.SqlClient.SqlDataAdapter> 填充 **Orders** 表。 强类型化 **数据集** 定义 **datarelation**。  
  
```vb  
' Assumes a valid SqlConnection object named connection.  
Dim customerAdapter As SqlDataAdapter = New SqlDataAdapter( _  
    "SELECT CustomerID, CompanyName, Phone FROM Customers", &  
    connection)  
Dim orderAdapter As SqlDataAdapter = New SqlDataAdapter( _  
    "SELECT OrderID, CustomerID, EmployeeID, OrderAdapter FROM Orders", &  
    connection)  
  
' Populate a strongly typed DataSet.  
connection.Open()  
Dim customers As CustomerDataSet = New CustomerDataSet()  
customerAdapter.Fill(customers, "Customers")  
orderAdapter.Fill(customers, "Orders")  
connection.Close()  
  
' Add a strongly typed event.  
AddHandler customers.Customers.CustomerChanged, &  
    New CustomerDataSet.CustomerChangeEventHandler( _  
    AddressOf OnCustomerChanged)  
  
' Add a strongly typed DataRow.  
Dim newCustomer As CustomerDataSet.Customer = _  
    customers.Customers.NewCustomer()  
newCustomer.CustomerID = "NEW01"  
newCustomer.CompanyName = "My New Company"  
customers.Customers.AddCustomer(newCustomer)  
  
' Navigate the child relation.  
Dim customer As CustomerDataSet.Customer  
Dim order As CustomerDataSet.Order  
  
For Each customer In customers.Customers  
  Console.WriteLine(customer.CustomerID)  
  For Each order In customer.GetOrders()  
    Console.WriteLine(vbTab & order.OrderID)  
  Next  
Next  
  
Private Shared Sub OnCustomerChanged( _  
    sender As Object, e As CustomerDataSet.CustomerChangeEvent)  
  
End Sub  
```  
  
```csharp  
// Assumes a valid SqlConnection object named connection.  
SqlDataAdapter customerAdapter = new SqlDataAdapter(  
    "SELECT CustomerID, CompanyName, Phone FROM Customers",  
    connection);  
SqlDataAdapter orderAdapter = new SqlDataAdapter(  
    "SELECT OrderID, CustomerID, EmployeeID, OrderAdapter FROM Orders",
    connection);  
  
// Populate a strongly typed DataSet.  
connection.Open();  
CustomerDataSet customers = new CustomerDataSet();  
customerAdapter.Fill(customers, "Customers");  
orderAdapter.Fill(customers, "Orders");  
connection.Close();  
  
// Add a strongly typed event.  
customers.Customers.CustomerChanged += new
  CustomerDataSet.CustomerChangeEventHandler(OnCustomerChanged);  
  
// Add a strongly typed DataRow.  
CustomerDataSet.Customer newCustomer =
    customers.Customers.NewCustomer();  
newCustomer.CustomerID = "NEW01";  
newCustomer.CompanyName = "My New Company";  
customers.Customers.AddCustomer(newCustomer);  
  
// Navigate the child relation.  
foreach(CustomerDataSet.Customer customer in customers.Customers)  
{  
  Console.WriteLine(customer.CustomerID);  
  foreach(CustomerDataSet.Order order in customer.GetOrders())  
    Console.WriteLine("\t" + order.OrderID);  
}  
  
protected static void OnCustomerChanged(object sender, CustomerDataSet.CustomerChangeEvent e)  
    {  
  
    }  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataColumnCollection>
- <xref:System.Data.DataSet>
- [类型化数据集](typed-datasets.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
