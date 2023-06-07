---
title: XML 序列化示例
description: 这些代码示例显示了高级方案，包括如何使用 XML 序列化生成符合 XML 架构文档的 XML 流。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML serialization, examples
- arrays, serializing
- ICollection interface, serializing
- XmlSerializer class, serializing
- serialization, examples
- DataSet class, serializing
- XML Schema, serializing
ms.assetid: eec46337-9696-435b-a375-dc5effae6992
ms.openlocfilehash: 98cc4a983c9703e6c5ab132f6110a327c6081b6c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289624"
---
# <a name="examples-of-xml-serialization"></a>XML 序列化示例

XML 序列化可以采用从简单到复杂的多种形式。 例如，可以序列化只包含公共字段和公共属性的类，如 [XML 序列化简介](introducing-xml-serialization.md)中所示。 下面的代码示例讨论各种高级方案，包括如何使用 XML 序列化生成符合特定 XML 架构 (XSD) 文档的 XML 流。

## <a name="serializing-a-dataset"></a>序列化数据集

除了序列化公共类的实例外，还可序列化 <xref:System.Data.DataSet> 的实例，如下面的代码示例所示。

```vb
Private Sub SerializeDataSet(filename As String)
    Dim ser As XmlSerializer = new XmlSerializer(GetType(DataSet))
    ' Creates a DataSet; adds a table, column, and ten rows.
    Dim ds As DataSet = new DataSet("myDataSet")
    Dim t As DataTable = new DataTable("table1")
    Dim c As DataColumn = new DataColumn("thing")
    t.Columns.Add(c)
    ds.Tables.Add(t)
    Dim r As DataRow
    Dim i As Integer
    for i = 0 to 10
        r = t.NewRow()
        r(0) = "Thing " &  i
        t.Rows.Add(r)
    Next
    Dim writer As TextWriter = new StreamWriter(filename)
    ser.Serialize(writer, ds)
    writer.Close()
End Sub
```

```csharp
private void SerializeDataSet(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(DataSet));

    // Creates a DataSet; adds a table, column, and ten rows.
    DataSet ds = new DataSet("myDataSet");
    DataTable t = new DataTable("table1");
    DataColumn c = new DataColumn("thing");
    t.Columns.Add(c);
    ds.Tables.Add(t);
    DataRow r;
    for(int i = 0; i<10;i++){
        r = t.NewRow();
        r[0] = "Thing " + i;
        t.Rows.Add(r);
    }
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, ds);
    writer.Close();
}
```

## <a name="serializing-an-xmlelement-and-xmlnode"></a>序列化 XmlElement 和 XmlNode

还可序列化 <xref:System.Xml.XmlElement> 或 <xref:System.Xml.XmlNode> 类的实例，如下面的代码示例所示。

```vb
private Sub SerializeElement(filename As String)
    Dim ser As XmlSerializer = new XmlSerializer(GetType(XmlElement))
    Dim myElement As XmlElement = _
    new XmlDocument().CreateElement("MyElement", "ns")
    myElement.InnerText = "Hello World"
    Dim writer As TextWriter = new StreamWriter(filename)
    ser.Serialize(writer, myElement)
    writer.Close()
End Sub

Private Sub SerializeNode(filename As String)
    Dim ser As XmlSerializer = _
    new XmlSerializer(GetType(XmlNode))
    Dim myNode As XmlNode = new XmlDocument(). _
    CreateNode(XmlNodeType.Element, "MyNode", "ns")
    myNode.InnerText = "Hello Node"
    Dim writer As TextWriter = new StreamWriter(filename)
    ser.Serialize(writer, myNode)
    writer.Close()
End Sub
```

```csharp
private void SerializeElement(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(XmlElement));
    XmlElement myElement=
    new XmlDocument().CreateElement("MyElement", "ns");
    myElement.InnerText = "Hello World";
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, myElement);
    writer.Close();
}

private void SerializeNode(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(XmlNode));
    XmlNode myNode= new XmlDocument().
    CreateNode(XmlNodeType.Element, "MyNode", "ns");
    myNode.InnerText = "Hello Node";
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, myNode);
    writer.Close();
}
```

## <a name="serializing-a-class-that-contains-a-field-returning-a-complex-object"></a>序列化包含返回复杂对象的字段的类

如果属性或字段返回一个复杂对象（如数组或类实例），则 <xref:System.Xml.Serialization.XmlSerializer> 将其转换为嵌套在主 XML 文档内的元素。 例如，以下代码示例中的第一个类返回第二个类的实例。

```vb
Public Class PurchaseOrder
    Public MyAddress As Address
End Class

Public Class Address
    Public FirstName As String
End Class
```

```csharp
public class PurchaseOrder
{
    public Address MyAddress;
}
public class Address
{
    public string FirstName;
}
```

已序列化的 XML 输出可能如下所示。

```xml
<PurchaseOrder>
    <MyAddress>
        <FirstName>George</FirstName>
    </MyAddress>
</PurchaseOrder>
```

## <a name="serializing-an-array-of-objects"></a>序列化对象数组

还可以序列化返回对象数组的字段，如下面的代码示例所示。

```vb
Public Class PurchaseOrder
    public ItemsOrders () As Item
End Class

Public Class Item
    Public ItemID As String
    Public ItemPrice As decimal
End Class
```

```csharp
public class PurchaseOrder
{
    public Item [] ItemsOrders;
}

public class Item
{
    public string ItemID;
    public decimal ItemPrice;
}
```

如果两个项已排序，则已序列化的类实例可能如下所示。

```xml
<PurchaseOrder xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <ItemsOrders>
        <Item>
            <ItemID>aaa111</ItemID>
            <ItemPrice>34.22</ItemPrice>
        </Item>
        <Item>
            <ItemID>bbb222</ItemID>
            <ItemPrice>2.89</ItemPrice>
        </Item>
    </ItemsOrders>
</PurchaseOrder>
```

## <a name="serializing-a-class-that-implements-the-icollection-interface"></a>序列化实现 ICollection 接口的类

您可以通过实现 <xref:System.Collections.ICollection> 接口创建自己的集合类，并使用 <xref:System.Xml.Serialization.XmlSerializer> 序列化这些类的实例。 请注意，当类实现 <xref:System.Collections.ICollection> 接口时，只序列化该类包含的集合， 而不会序列化添加至该类的任何公共属性或字段。 该类必须包含 Add 方法和 Item 属性（C# 索引器）才能被序列化 。

```vb
Imports System.Collections
Imports System.IO
Imports System.Xml.Serialization

Public Class Test
    Shared Sub Main()
        Dim t As Test= new Test()
        t.SerializeCollection("coll.xml")
    End Sub

    Private Sub SerializeCollection(filename As String)
        Dim Emps As Employees  = new Employees()
        ' Note that only the collection is serialized -- not the
        ' CollectionName or any other public property of the class.
        Emps.CollectionName = "Employees"
        Dim John100 As Employee = new Employee("John", "100xxx")
        Emps.Add(John100)
        Dim x As XmlSerializer = new XmlSerializer(GetType(Employees))
        Dim writer As TextWriter = new StreamWriter(filename)
        x.Serialize(writer, Emps)
        writer.Close()
    End Sub
End Class

Public Class Employees
    Implements ICollection
    Public CollectionName As String
    Private empArray As ArrayList = new ArrayList()

    Public ReadOnly Default Overloads _
    Property Item(index As Integer) As Employee
        get
        return CType (empArray(index), Employee)
        End Get
    End Property

    Public Sub CopyTo(a As Array, index As Integer) _
    Implements ICollection.CopyTo
        empArray.CopyTo(a, index)
    End Sub

    Public ReadOnly Property Count () As integer Implements _
    ICollection.Count
        get
            Count = empArray.Count
        End Get

    End Property

    Public ReadOnly Property SyncRoot ()As Object _
    Implements ICollection.SyncRoot
        get
        return me
        End Get
    End Property

    Public ReadOnly Property IsSynchronized () As Boolean _
    Implements ICollection.IsSynchronized
        get
        return false
        End Get
    End Property

    Public Function GetEnumerator() As IEnumerator _
    Implements IEnumerable.GetEnumerator

        return empArray.GetEnumerator()
    End Function

    Public Function Add(newEmployee As Employee) As Integer
        empArray.Add(newEmployee)
        return empArray.Count
    End Function
End Class

Public Class Employee
    Public EmpName As String
    Public EmpID As String

    Public Sub New ()
    End Sub

    Public Sub New (newName As String , newID As String )
        EmpName = newName
        EmpID = newID
    End Sub
End Class
```

```csharp
using System;
using System.Collections;
using System.IO;
using System.Xml.Serialization;

public class Test {
    static void Main(){
        Test t = new Test();
        t.SerializeCollection("coll.xml");
    }

    private void SerializeCollection(string filename){
        Employees Emps = new Employees();
        // Note that only the collection is serialized -- not the
        // CollectionName or any other public property of the class.
        Emps.CollectionName = "Employees";
        Employee John100 = new Employee("John", "100xxx");
        Emps.Add(John100);
        XmlSerializer x = new XmlSerializer(typeof(Employees));
        TextWriter writer = new StreamWriter(filename);
        x.Serialize(writer, Emps);
    }
}
public class Employees:ICollection {
    public string CollectionName;
    private ArrayList empArray = new ArrayList();

    public Employee this[int index]{
        get{return (Employee) empArray[index];}
    }

    public void CopyTo(Array a, int index){
        empArray.CopyTo(a, index);
    }
    public int Count{
        get{return empArray.Count;}
    }
    public object SyncRoot{
        get{return this;}
    }
    public bool IsSynchronized{
        get{return false;}
    }
    public IEnumerator GetEnumerator(){
        return empArray.GetEnumerator();
    }

    public void Add(Employee newEmployee){
        empArray.Add(newEmployee);
    }
}

public class Employee {
    public string EmpName;
    public string EmpID;
    public Employee(){}
    public Employee(string empName, string empID){
        EmpName = empName;
        EmpID = empID;
    }
}
```

## <a name="purchase-order-example"></a>订单示例

您可以将下面的示例代码剪切并粘贴到以 .cs 或 .vb 文件扩展名重命名的文本文件中。 使用 C# 或 Visual Basic 编译器编译该文件， 然后使用生成的可执行文件的名称运行该文件。

此示例使用一个简单方案演示如何创建对象的实例，并使用 <xref:System.Xml.Serialization.XmlSerializer.Serialize%2A> 方法将该对象实例序列化为文件流。 将 XML 流保存到文件，然后使用 <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> 方法读回该文件，并将其重新构造为原始对象的副本。

在此示例中，对名为 `PurchaseOrder` 的类进行序列化，然后进行反序列化。 另一个名为 `Address` 的类也包含在内，因为名为 `ShipTo` 的公共字段必须设置为 `Address`。 同样，`OrderedItem` 类也包含在内，因为 `OrderedItem` 对象的数组必须设置为 `OrderedItems` 字段。 最后，名为 `Test` 的类包含序列化和反序列化这些类的代码。

`CreatePO` 方法创建 `PurchaseOrder`、`Address` 和 `OrderedItem` 类对象，并设置公共字段值。 该方法还构造 <xref:System.Xml.Serialization.XmlSerializer> 类的实例，该类用于序列化和反序列化 `PurchaseOrder`。 请注意，代码传递的是将序列化为构造函数的类的类型。 代码还创建可用于将 XML 流写入 XML 文档的 `FileStream`。

`ReadPo` 方法稍简单一些。 它只创建要反序列化的对象并读出它们的值。 与 `CreatePo` 方法一样，必须先构造 <xref:System.Xml.Serialization.XmlSerializer>，并将要反序列化的类的类型传递给该构造函数。 此外，还需要使用 <xref:System.IO.FileStream> 读取 XML 文档。 要反序列化对象，请调用带有 <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> 参数的 <xref:System.IO.FileStream> 方法。 已反序列化的对象必须强制转换为 `PurchaseOrder` 类型的对象变量。 然后代码读取已反序列化的 `PurchaseOrder` 的值。 请注意，您还可以读取 PO.xml 文件，创建该文件是为了查看实际的 XML 输出。

```vb
Imports System.IO
Imports System.Xml
Imports System.Xml.Serialization
Imports Microsoft.VisualBasic

' The XmlRoot attribute allows you to set an alternate name
' (PurchaseOrder) for the XML element and its namespace. By
' default, the XmlSerializer uses the class name. The attribute
' also allows you to set the XML namespace for the element. Lastly,
' the attribute sets the IsNullable property, which specifies whether
' the xsi:null attribute appears if the class instance is set to
' a null reference.
<XmlRoot("PurchaseOrder", _
 Namespace := "http://www.cpandl.com", IsNullable := False)> _
Public Class PurchaseOrder
    Public ShipTo As Address
    Public OrderDate As String
    ' The XmlArrayAttribute changes the XML element name
    ' from the default of "OrderedItems" to "Items".
    <XmlArray("Items")> _
    Public OrderedItems() As OrderedItem
    Public SubTotal As Decimal
    Public ShipCost As Decimal
    Public TotalCost As Decimal
End Class

Public Class Address
    ' The XmlAttribute attribute instructs the XmlSerializer to serialize the
    ' Name field as an XML attribute instead of an XML element (XML element is
    ' the default behavior).
    <XmlAttribute()> _
    Public Name As String
    Public Line1 As String

    ' Setting the IsNullable property to false instructs the
    ' XmlSerializer that the XML attribute will not appear if
    ' the City field is set to a null reference.
    <XmlElement(IsNullable := False)> _
    Public City As String
    Public State As String
    Public Zip As String
End Class

Public Class OrderedItem
    Public ItemName As String
    Public Description As String
    Public UnitPrice As Decimal
    Public Quantity As Integer
    Public LineTotal As Decimal

    ' Calculate is a custom method that calculates the price per item
    ' and stores the value in a field.
    Public Sub Calculate()
    LineTotal = UnitPrice * Quantity
    End Sub
End Class

Public Class Test
        Public Shared Sub Main()
    ' Read and write purchase orders.
    Dim t As New Test()
    t.CreatePO("po.xml")
    t.ReadPO("po.xml")
    End Sub

    Private Sub CreatePO(filename As String)
        ' Creates an instance of the XmlSerializer class;
        ' specifies the type of object to serialize.
        Dim serializer As New XmlSerializer(GetType(PurchaseOrder))
        Dim writer As New StreamWriter(filename)
        Dim po As New PurchaseOrder()

        ' Creates an address to ship and bill to.
        Dim billAddress As New Address()
        billAddress.Name = "Teresa Atkinson"
        billAddress.Line1 = "1 Main St."
        billAddress.City = "AnyTown"
        billAddress.State = "WA"
        billAddress.Zip = "00000"
        ' Set ShipTo and BillTo to the same addressee.
        po.ShipTo = billAddress
        po.OrderDate = System.DateTime.Now.ToLongDateString()

        ' Creates an OrderedItem.
        Dim i1 As New OrderedItem()
        i1.ItemName = "Widget S"
        i1.Description = "Small widget"
        i1.UnitPrice = CDec(5.23)
        i1.Quantity = 3
        i1.Calculate()

        ' Inserts the item into the array.
        Dim items(0) As OrderedItem
        items(0) = i1
        po.OrderedItems = items
        ' Calculates the total cost.
        Dim subTotal As New Decimal()
        Dim oi As OrderedItem
        For Each oi In  items
            subTotal += oi.LineTotal
        Next oi
        po.SubTotal = subTotal
        po.ShipCost = CDec(12.51)
        po.TotalCost = po.SubTotal + po.ShipCost
        ' Serializes the purchase order, and close the TextWriter.
        serializer.Serialize(writer, po)
        writer.Close()
    End Sub

    Protected Sub ReadPO(filename As String)
        ' Creates an instance of the XmlSerializer class;
        ' specifies the type of object to be deserialized.
        Dim serializer As New XmlSerializer(GetType(PurchaseOrder))
        ' If the XML document has been altered with unknown
        ' nodes or attributes, handles them with the
        ' UnknownNode and UnknownAttribute events.
        AddHandler serializer.UnknownNode, AddressOf serializer_UnknownNode
        AddHandler serializer.UnknownAttribute, AddressOf _
        serializer_UnknownAttribute

        ' A FileStream is needed to read the XML document.
        Dim fs As New FileStream(filename, FileMode.Open)
        ' Declare an object variable of the type to be deserialized.
        Dim po As PurchaseOrder
        ' Uses the Deserialize method to restore the object's state
        ' with data from the XML document.
        po = CType(serializer.Deserialize(fs), PurchaseOrder)
        ' Reads the order date.
        Console.WriteLine(("OrderDate: " & po.OrderDate))

        ' Reads the shipping address.
        Dim shipTo As Address = po.ShipTo
        ReadAddress(shipTo, "Ship To:")
        ' Reads the list of ordered items.
        Dim items As OrderedItem() = po.OrderedItems
        Console.WriteLine("Items to be shipped:")
        Dim oi As OrderedItem
        For Each oi In items
            Console.WriteLine((ControlChars.Tab & oi.ItemName & _
            ControlChars.Tab & _
                oi.Description & ControlChars.Tab & oi.UnitPrice & _
                ControlChars.Tab & _
                oi.Quantity & ControlChars.Tab & oi.LineTotal))
        Next oi
        ' Reads the subtotal, shipping cost, and total cost.
        Console.WriteLine((ControlChars.Cr & New String _
        (ControlChars.Tab, 5) & _
        " Subtotal" & ControlChars.Tab & po.SubTotal & ControlChars.Cr & _
        New String(ControlChars.Tab, 5) & " Shipping" & ControlChars.Tab & _
        po.ShipCost & ControlChars.Cr &  New String(ControlChars.Tab, 5) & _
        " Total" & New String(ControlChars.Tab, 2) & po.TotalCost))
    End Sub

    Protected Sub ReadAddress(a As Address, label As String)
        ' Reads the fields of the Address.
        Console.WriteLine(label)
        Console.Write((ControlChars.Tab & a.Name & ControlChars.Cr & _
        ControlChars.Tab & a.Line1 & ControlChars.Cr & ControlChars.Tab & _
        a.City & ControlChars.Tab & a.State & ControlChars.Cr & _
        ControlChars.Tab & a.Zip & ControlChars.Cr))
    End Sub

    Protected Sub serializer_UnknownNode(sender As Object, e As _
    XmlNodeEventArgs)
        Console.WriteLine(("Unknown Node:" & e.Name & _
        ControlChars.Tab & e.Text))
    End Sub

    Protected Sub serializer_UnknownAttribute(sender As Object, _
    e As XmlAttributeEventArgs)
        Dim attr As System.Xml.XmlAttribute = e.Attr
        Console.WriteLine(("Unknown attribute " & attr.Name & "='" & _
        attr.Value & "'"))
    End Sub 'serializer_UnknownAttribute
End Class 'Test
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.Serialization;

// The XmlRoot attribute allows you to set an alternate name
// (PurchaseOrder) for the XML element and its namespace. By
// default, the XmlSerializer uses the class name. The attribute
// also allows you to set the XML namespace for the element. Lastly,
// the attribute sets the IsNullable property, which specifies whether
// the xsi:null attribute appears if the class instance is set to
// a null reference.
[XmlRoot("PurchaseOrder", Namespace="http://www.cpandl.com",
IsNullable = false)]
public class PurchaseOrder
{
    public Address ShipTo;
    public string OrderDate;
    // The XmlArray attribute changes the XML element name
    // from the default of "OrderedItems" to "Items".
    [XmlArray("Items")]
    public OrderedItem[] OrderedItems;
    public decimal SubTotal;
    public decimal ShipCost;
    public decimal TotalCost;
}

public class Address
{
    // The XmlAttribute attribute instructs the XmlSerializer to serialize the
    // Name field as an XML attribute instead of an XML element (XML element is
    // the default behavior).
    [XmlAttribute]
    public string Name;
    public string Line1;

    // Setting the IsNullable property to false instructs the
    // XmlSerializer that the XML attribute will not appear if
    // the City field is set to a null reference.
    [XmlElement(IsNullable = false)]
    public string City;
    public string State;
    public string Zip;
}

public class OrderedItem
{
    public string ItemName;
    public string Description;
    public decimal UnitPrice;
    public int Quantity;
    public decimal LineTotal;

    // Calculate is a custom method that calculates the price per item
    // and stores the value in a field.
    public void Calculate()
    {
        LineTotal = UnitPrice * Quantity;
    }
}

public class Test
{
    public static void Main()
    {
        // Read and write purchase orders.
        Test t = new Test();
        t.CreatePO("po.xml");
        t.ReadPO("po.xml");
    }

    private void CreatePO(string filename)
    {
        // Creates an instance of the XmlSerializer class;
        // specifies the type of object to serialize.
        XmlSerializer serializer =
        new XmlSerializer(typeof(PurchaseOrder));
        TextWriter writer = new StreamWriter(filename);
        PurchaseOrder po=new PurchaseOrder();

        // Creates an address to ship and bill to.
        Address billAddress = new Address();
        billAddress.Name = "Teresa Atkinson";
        billAddress.Line1 = "1 Main St.";
        billAddress.City = "AnyTown";
        billAddress.State = "WA";
        billAddress.Zip = "00000";
        // Sets ShipTo and BillTo to the same addressee.
        po.ShipTo = billAddress;
        po.OrderDate = System.DateTime.Now.ToLongDateString();

        // Creates an OrderedItem.
        OrderedItem i1 = new OrderedItem();
        i1.ItemName = "Widget S";
        i1.Description = "Small widget";
        i1.UnitPrice = (decimal) 5.23;
        i1.Quantity = 3;
        i1.Calculate();

        // Inserts the item into the array.
        OrderedItem [] items = {i1};
        po.OrderedItems = items;
        // Calculate the total cost.
        decimal subTotal = new decimal();
        foreach(OrderedItem oi in items)
        {
            subTotal += oi.LineTotal;
        }
        po.SubTotal = subTotal;
        po.ShipCost = (decimal) 12.51;
        po.TotalCost = po.SubTotal + po.ShipCost;
        // Serializes the purchase order, and closes the TextWriter.
        serializer.Serialize(writer, po);
        writer.Close();
    }

    protected void ReadPO(string filename)
    {
        // Creates an instance of the XmlSerializer class;
        // specifies the type of object to be deserialized.
        XmlSerializer serializer = new XmlSerializer(typeof(PurchaseOrder));
        // If the XML document has been altered with unknown
        // nodes or attributes, handles them with the
        // UnknownNode and UnknownAttribute events.
        serializer.UnknownNode+= new
        XmlNodeEventHandler(serializer_UnknownNode);
        serializer.UnknownAttribute+= new
        XmlAttributeEventHandler(serializer_UnknownAttribute);

        // A FileStream is needed to read the XML document.
        FileStream fs = new FileStream(filename, FileMode.Open);
        // Declares an object variable of the type to be deserialized.
        PurchaseOrder po;
        // Uses the Deserialize method to restore the object's state
        // with data from the XML document. */
        po = (PurchaseOrder) serializer.Deserialize(fs);
        // Reads the order date.
        Console.WriteLine ("OrderDate: " + po.OrderDate);

        // Reads the shipping address.
        Address shipTo = po.ShipTo;
        ReadAddress(shipTo, "Ship To:");
        // Reads the list of ordered items.
        OrderedItem [] items = po.OrderedItems;
        Console.WriteLine("Items to be shipped:");
        foreach(OrderedItem oi in items)
        {
            Console.WriteLine("\t"+
            oi.ItemName + "\t" +
            oi.Description + "\t" +
            oi.UnitPrice + "\t" +
            oi.Quantity + "\t" +
            oi.LineTotal);
        }
        // Reads the subtotal, shipping cost, and total cost.
        Console.WriteLine(
        "\n\t\t\t\t\t Subtotal\t" + po.SubTotal +
        "\n\t\t\t\t\t Shipping\t" + po.ShipCost +
        "\n\t\t\t\t\t Total\t\t" + po.TotalCost
        );
    }

    protected void ReadAddress(Address a, string label)
    {
        // Reads the fields of the Address.
        Console.WriteLine(label);
        Console.Write("\t"+
        a.Name +"\n\t" +
        a.Line1 +"\n\t" +
        a.City +"\t" +
        a.State +"\n\t" +
        a.Zip +"\n");
    }

    protected void serializer_UnknownNode
    (object sender, XmlNodeEventArgs e)
    {
        Console.WriteLine("Unknown Node:" +   e.Name + "\t" + e.Text);
    }

    protected void serializer_UnknownAttribute
    (object sender, XmlAttributeEventArgs e)
    {
        System.Xml.XmlAttribute attr = e.Attr;
        Console.WriteLine("Unknown attribute " +
        attr.Name + "='" + attr.Value + "'");
    }
}
```

XML 输出可能如下所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<PurchaseOrder xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://www.cpandl.com">
    <ShipTo Name="Teresa Atkinson">
        <Line1>1 Main St.</Line1>
        <City>AnyTown</City>
        <State>WA</State>
        <Zip>00000</Zip>
    </ShipTo>
    <OrderDate>Wednesday, June 27, 2001</OrderDate>
    <Items>
        <OrderedItem>
            <ItemName>Widget S</ItemName>
            <Description>Small widget</Description>
            <UnitPrice>5.23</UnitPrice>
            <Quantity>3</Quantity>
            <LineTotal>15.69</LineTotal>
        </OrderedItem>
    </Items>
    <SubTotal>15.69</SubTotal>
    <ShipCost>12.51</ShipCost>
    <TotalCost>28.2</TotalCost>
</PurchaseOrder>
```

## <a name="see-also"></a>请参阅

- [XML 序列化简介](introducing-xml-serialization.md)
- [使用属性控制 XML 序列化](controlling-xml-serialization-using-attributes.md)
- [用来控制 XML 序列化的属性](attributes-that-control-xml-serialization.md)
- [XmlSerializer 类](xref:System.Xml.Serialization.XmlSerializer)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
