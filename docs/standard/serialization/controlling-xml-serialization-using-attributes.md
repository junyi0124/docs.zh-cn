---
title: 使用属性控制 XML 序列化
description: 使用属性可以控制对象的 XML 序列化，还可以利用同一组类创建其他 XML 流。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- classes, serializing
- XML serialization, examples
- derived classes, serializing
- arrays, serializing
- XML serialization, attributes
- preventing serialization
- attributes [.NET], XML serialization
- serialization, examples
- serialization, attributes
ms.assetid: 47d4c39d-30e1-4c7b-8a2e-301325390647
ms.openlocfilehash: 36456d373b7482c4a4267b69e031c1e45baa9abb
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93281782"
---
# <a name="control-xml-serialization-using-attributes"></a>使用属性控制 XML 序列化

使用属性可以控制对象的 XML 序列化，还可以利用同一组类创建其他 XML 流。 有关创建其他 XML 流的详细信息，请参阅[如何：指定 XML 流的替代元素名称](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)。

> [!NOTE]
> 如果生成的 XML 必须符合万维网联合会 (W3C) 文档[简单对象访问协议 (SOAP) 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/) 第 5 节的内容，则需使用[控制编码的 SOAP 序列化的特性](attributes-that-control-encoded-soap-serialization.md)中所列的特性。

默认情况下，XML 元素名称由类或成员名称确定。 在名为 `Book` 的简单类中，名为 `ISBN` 的字段将生成 XML 元素标记 \<ISBN>，如下面的示例所示。

```vb
Public Class Book
    Public ISBN As String
End Class
' When an instance of the Book class is serialized, it might
' produce this XML:
' <ISBN>1234567890</ISBN>.
```

```csharp
public class Book
{
    public string ISBN;
}
// When an instance of the Book class is serialized, it might
// produce this XML:
// <ISBN>1234567890</ISBN>.
```

若要重新命名元素，可以更改这种默认行为。 下面的代码演示属性 (Attribute) 如何通过设置 <xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> 的 <xref:System.Xml.Serialization.XmlElementAttribute> 属性 (Property) 实现此目的。

```vb
Public Class TaxRates
   < XmlElement(ElementName = "TaxRate")> _
    Public ReturnTaxRate As Decimal
End Class
```

```csharp
public class TaxRates {
    [XmlElement(ElementName = "TaxRate")]
    public decimal ReturnTaxRate;
}
```

有关属性的详细信息，请参阅[属性](../attributes/index.md)。 有关控制 XML 序列化的特性的列表，请参阅[控制 XML 序列化的特性](attributes-that-control-xml-serialization.md)。

## <a name="controlling-array-serialization"></a>控制数组序列化

<xref:System.Xml.Serialization.XmlArrayAttribute> 和 <xref:System.Xml.Serialization.XmlArrayItemAttribute> 属性旨在用于控制数组的序列化。 使用这些特性可以控制元素名称、命名空间以及 XML 架构 (XSD) 数据类型（在万维网联合会 [www.w3.org] 文档“XML 架构第 2 部分：数据类型”中进行了定义）。 此外，还可以指定数组所能包含的类型。

对于序列化数组时生成的封闭 XML 元素，其属性将由 <xref:System.Xml.Serialization.XmlArrayAttribute> 确定。 例如，默认情况下，序列化下面的数组时，将会生成名为 `Employees` 的 XML 元素。 `Employees` 元素将包含在数组类型 `Employee` 之后命名的一系列元素。

```vb
Public Class Group
    Public Employees() As Employee
End Class
Public Class Employee
    Public Name As String
End Class
```

```csharp
public class Group {
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
```

序列化实例可能如下所示。

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
</Employees>
</Group>
```

通过应用 <xref:System.Xml.Serialization.XmlArrayAttribute>，可以按照以下方式更改 XML 元素的名称。

```vb
Public Class Group
    <XmlArray("TeamMembers")> _
    Public Employees() As Employee
End Class
```

```csharp
public class Group {
    [XmlArray("TeamMembers")]
    public Employee[] Employees;
}
```

生成的 XML 可能如下所示。

```xml
<Group>
<TeamMembers>
    <Employee>
        <Name>Haley</Name>
    </Employee>
</TeamMembers>
</Group>
```

另一方面，<xref:System.Xml.Serialization.XmlArrayItemAttribute> 可以控制如何序列化数组中包含的项。 请注意，该特性将应用于返回数组的字段。

```vb
Public Class Group
    <XmlArrayItem("MemberName")> _
    Public Employee() As Employees
End Class
```

```csharp
public class Group {
    [XmlArrayItem("MemberName")]
    public Employee[] Employees;
}
```

生成的 XML 可能如下所示。

```xml
<Group>
<Employees>
    <MemberName>Haley</MemberName>
</Employees>
</Group>
```

## <a name="serializing-derived-classes"></a>序列化派生类

<xref:System.Xml.Serialization.XmlArrayItemAttribute> 的另一种用法是，允许序列化派生类。 例如，可将派生自 `Manager` 的另一个名为 `Employee` 的类添加至上一示例中。 如果没有应用 <xref:System.Xml.Serialization.XmlArrayItemAttribute>，代码将在运行时失败，原因是无法识别派生类类型。 若要解决这个问题，每次为每个可接受类型（基类和派生类）设置 <xref:System.Xml.Serialization.XmlArrayItemAttribute.Type%2A> 属性时，需要应用该特性两次。

```vb
Public Class Group
    <XmlArrayItem(Type:=GetType(Employee)), _
    XmlArrayItem(Type:=GetType(Manager))> _
    Public Employees() As Employee
End Class
Public Class Employee
    Public Name As String
End Class
Public Class Manager
Inherits Employee
    Public Level As Integer
End Class
```

```csharp
public class Group {
    [XmlArrayItem(Type = typeof(Employee)),
    XmlArrayItem(Type = typeof(Manager))]
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
public class Manager:Employee {
    public int Level;
}
```

序列化实例可能如下所示。

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
    <Employee xsi:type = "Manager">
        <Name>Ann</Name>
        <Level>3</Level>
    </Employee>
</Employees>
</Group>
```

## <a name="serializing-an-array-as-a-sequence-of-elements"></a>将数组作为元素序列进行序列化

通过将 <xref:System.Xml.Serialization.XmlElementAttribute> 应用于返回数组的字段，还可以将该数组作为 XML 元素的平面序列进行序列化，如下所示。

```vb
Public Class Group
    <XmlElement> _
    Public Employees() As Employee
End Class
```

```csharp
public class Group {
    [XmlElement]
    public Employee[] Employees;
}
```

序列化实例可能如下所示。

```xml
<Group>
<Employees>
    <Name>Haley</Name>
</Employees>
<Employees>
    <Name>Noriko</Name>
</Employees>
<Employees>
    <Name>Marco</Name>
</Employees>
</Group>
```

区别两种 XML 流的另一个方法是，使用 XML 架构定义工具，从编译好的代码生成 XML 架构 (XSD) 文档文件。 （有关使用该工具的详细信息，请参见[XML 架构定义工具和 XML 序列化](the-xml-schema-definition-tool-and-xml-serialization.md)。）没有将属性应用于字段时，架构会以下列方式描述元素。

```xml
<xs:element minOccurs="0" maxOccurs ="1" name="Employees" type="ArrayOfEmployee" />
```

将 <xref:System.Xml.Serialization.XmlElementAttribute> 应用于字段时，生成的架构会以下列方式描述元素。

```xml
<xs:element minOccurs="0" maxOccurs="unbounded" name="Employees" type="Employee" />
```

## <a name="serializing-an-arraylist"></a>序列化 ArrayList

<xref:System.Collections.ArrayList> 类可能包含各种不同对象的集合。 因此，可以按照使用数组的类似方式使用 <xref:System.Collections.ArrayList>。 您可以创建返回单个 <xref:System.Collections.ArrayList> 的字段，而不用创建返回类型化对象的数组的字段。 但是，与数组相同的是，必须将 <xref:System.Xml.Serialization.XmlSerializer> 包含的对象的类型告知 <xref:System.Collections.ArrayList>。 为此，需要为该字段分配 <xref:System.Xml.Serialization.XmlElementAttribute> 的多个实例，如下面的示例所示。

```vb
Public Class Group
    <XmlElement(Type:=GetType(Employee)), _
    XmlElement(Type:=GetType(Manager))> _
    Public Info As ArrayList
End Class
```

```csharp
public class Group {
    [XmlElement(Type = typeof(Employee)),
    XmlElement(Type = typeof(Manager))]
    public ArrayList Info;
}
```

## <a name="controlling-serialization-of-classes-using-xmlrootattribute-and-xmltypeattribute"></a>使用 XmlRootAttribute 和 XmlTypeAttribute 控制类的序列化

能且只能应用于一个类的特性有下面两种：<xref:System.Xml.Serialization.XmlRootAttribute> 和 <xref:System.Xml.Serialization.XmlTypeAttribute>。 这两种特性非常相似。 <xref:System.Xml.Serialization.XmlRootAttribute> 只能应用于一个类：序列化时，该类表示 XML 文档的开始和结束元素，也就是根元素。 另一方面，<xref:System.Xml.Serialization.XmlTypeAttribute> 可以应用于任何一个类，包括根类。

例如，在上面的示例中，`Group` 类就是根类，而其所有的公共字段和属性变成 XML 文档中的 XML 元素。 因此，只能有一个根类。 通过应用 <xref:System.Xml.Serialization.XmlRootAttribute>，可以控制 <xref:System.Xml.Serialization.XmlSerializer> 所生成的 XML 流。 例如，可以更改元素名称和命名空间。

使用 <xref:System.Xml.Serialization.XmlTypeAttribute> 可以控制所生成 XML 的架构。 需要通过 XML Web services 发布架构时，这项功能很有用。 下面的示例将 <xref:System.Xml.Serialization.XmlTypeAttribute> 和 <xref:System.Xml.Serialization.XmlRootAttribute> 同时应用于同一个类。

```vb
<XmlRoot("NewGroupName"), _
XmlType("NewTypeName")> _
Public Class Group
    Public Employees() As Employee
End Class
```

```csharp
[XmlRoot("NewGroupName")]
[XmlType("NewTypeName")]
public class Group {
    public Employee[] Employees;
}
```

如果对该类进行编译，并且使用 XML 架构定义工具生成其架构，可能会找到下面描述 `Group` 的 XML。

```xml
<xs:element name="NewGroupName" type="NewTypeName" />
```

相比之下，如果是对该类的实例进行序列化，则只能在 XML 文档中找到 `NewGroupName`。

```xml
<NewGroupName>
    . . .
</NewGroupName>
```

## <a name="preventing-serialization-with-the-xmlignoreattribute"></a>使用 XmlIgnoreAttribute 防止序列化

可能会出现公共属性或字段无需进行序列化的情况。 例如，字段或属性可能用于包含元数据。 在这样的情况下，可将 <xref:System.Xml.Serialization.XmlIgnoreAttribute> 应用于该字段或属性，而 <xref:System.Xml.Serialization.XmlSerializer> 将跳过它。

## <a name="see-also"></a>请参阅

- [用来控制 XML 序列化的属性](attributes-that-control-xml-serialization.md)
- [用来控制编码的 SOAP 序列化的属性](attributes-that-control-encoded-soap-serialization.md)
- [XML 序列化简介](introducing-xml-serialization.md)
- [XML 序列化示例](examples-of-xml-serialization.md)
- [如何：指定 XML 流的替代元素名称](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
