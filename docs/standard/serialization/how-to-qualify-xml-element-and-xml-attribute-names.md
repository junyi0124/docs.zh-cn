---
title: 如何限定 XML 元素和 XML 属性名
description: 本文说明如何在 XML 文档中限定 XML 元素和 XML 属性的名称。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- qualifying XML names
- qualifying XML elements
- XML namespaces, qualifying elements and names in
ms.assetid: 44719f90-7e15-42e8-a9e2-282287e2b5bf
ms.openlocfilehash: 6c29e03d9ce28e5b0abc68a5d7e8d82f4485ac93
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83378417"
---
# <a name="how-to-qualify-xml-element-and-xml-attribute-names"></a>如何限定 XML 元素和 XML 属性名

<xref:System.Xml.Serialization.XmlSerializerNamespaces> 类的实例所包含的 XML 命名空间必须符合万维网联合会 (W3C) 规范，即 [XML 中的命名空间](https://www.w3.org/TR/REC-xml-names/)。

XML 命名空间提供了一种方法，用来限定 XML 文档中 XML 元素和 XML 特性的名称。 限定名由前缀和本地名称组成，两者之间用冒号分隔。 前缀仅用作占位符；它将映射到用于指定命名空间的 URI。 统一管理的 URI 命名空间和本地名称的组合能够产生保证为全局唯一的名称。

通过创建 `XmlSerializerNamespaces` 的实例，并向对象中添加命名空间对，可以指定在 XML 文档中使用的前缀。

## <a name="to-create-qualified-names-in-an-xml-document"></a>在 XML 文档中创建限定名称

1. 创建 `XmlSerializerNamespaces` 类的一个实例。

2. 将所有的前缀和命名空间对添加至 `XmlSerializerNamespaces`。

3. 对每个由 `System.Xml.Serialization` 序列化为 XML 文档的成员或类应用相应的 <xref:System.Xml.Serialization.XmlSerializer> 特性。

    可用的特性包括：<xref:System.Xml.Serialization.XmlAnyElementAttribute>、<xref:System.Xml.Serialization.XmlArrayAttribute>、<xref:System.Xml.Serialization.XmlArrayItemAttribute>、<xref:System.Xml.Serialization.XmlAttributeAttribute>、<xref:System.Xml.Serialization.XmlElementAttribute>、<xref:System.Xml.Serialization.XmlRootAttribute> 和 <xref:System.Xml.Serialization.XmlTypeAttribute>。

4. 将每个属性 (Attribute) 的 `Namespace` 属性 (Property) 设置为 `XmlSerializerNamespaces` 的命名空间值之一。

5. 将 `XmlSerializerNamespaces` 传递到 `Serialize` 的 `XmlSerializer` 方法。

## <a name="example"></a>示例

下面的示例创建一个 `XmlSerializerNamespaces`，并向对象添加两个前缀和命名空间对。 该代码创建了用来序列化 `XmlSerializer` 类实例的 `Books`， 并通过 `Serialize` 调用 `XmlSerializerNamespaces` 方法，使 XML 可以包含带前缀的命名空间。

```vb
Imports System.IO
Imports System.Xml
Imports System.Xml.Serialization

Public Module Program

    Public Sub Main()
        SerializeObject("XmlNamespaces.xml")
    End Sub

    Public Sub SerializeObject(filename As String)
        Dim mySerializer As New XmlSerializer(GetType(Books))
        ' Writing a file requires a TextWriter.
        Dim myWriter As New StreamWriter(filename)

        ' Creates an XmlSerializerNamespaces and adds two
        ' prefix-namespace pairs.
        Dim myNamespaces As New XmlSerializerNamespaces()
        myNamespaces.Add("books", "http://www.cpandl.com")
        myNamespaces.Add("money", "http://www.cohowinery.com")

        ' Creates a Book.
        Dim myBook As New Book()
        myBook.TITLE = "A Book Title"
        Dim myPrice As New Price()
        myPrice.price = CDec(9.95)
        myPrice.currency = "US Dollar"
        myBook.PRICE = myPrice
        Dim myBooks As New Books()
        myBooks.Book = myBook
        mySerializer.Serialize(myWriter, myBooks, myNamespaces)
        myWriter.Close()
    End Sub
End Module

Public Class Books
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public Book As Book
End Class

<XmlType([Namespace] := "http://www.cpandl.com")> _
Public Class Book
    <XmlElement([Namespace] := "http://www.cpandl.com")> _
    Public TITLE As String
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public PRICE As Price
End Class

Public Class Price
    <XmlAttribute([Namespace] := "http://www.cpandl.com")> _
    Public currency As String
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public price As Decimal
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.Serialization;

public class Program
{
    public static void Main()
    {
        SerializeObject("XmlNamespaces.xml");
    }

    public static void SerializeObject(string filename)
    {
        var mySerializer = new XmlSerializer(typeof(Books));
        // Writing a file requires a TextWriter.
        TextWriter myWriter = new StreamWriter(filename);

        // Creates an XmlSerializerNamespaces and adds two
        // prefix-namespace pairs.
        var myNamespaces = new XmlSerializerNamespaces();
        myNamespaces.Add("books", "http://www.cpandl.com");
        myNamespaces.Add("money", "http://www.cohowinery.com");

        // Creates a Book.
        var myBook = new Book();
        myBook.TITLE = "A Book Title";
        var myPrice = new Price();
        myPrice.price = (decimal) 9.95;
        myPrice.currency = "US Dollar";
        myBook.PRICE = myPrice;
        var myBooks = new Books();
        myBooks.Book = myBook;
        mySerializer.Serialize(myWriter, myBooks, myNamespaces);
        myWriter.Close();
    }
}

public class Books
{
    [XmlElement(Namespace = "http://www.cohowinery.com")]
    public Book Book;
}

[XmlType(Namespace ="http://www.cpandl.com")]
public class Book
{
    [XmlElement(Namespace = "http://www.cpandl.com")]
    public string TITLE;
    [XmlElement(Namespace ="http://www.cohowinery.com")]
    public Price PRICE;
}

public class Price
{
    [XmlAttribute(Namespace = "http://www.cpandl.com")]
    public string currency;
    [XmlElement(Namespace = "http://www.cohowinery.com")]
    public decimal price;
}
```

## <a name="see-also"></a>请参阅

- <xref:System.Xml.Serialization.XmlSerializer>
- [XML 架构定义工具和 XML 序列化](the-xml-schema-definition-tool-and-xml-serialization.md)
- [XML 序列化简介](introducing-xml-serialization.md)
- [XmlSerializer 类](xref:System.Xml.Serialization.XmlSerializer)
- [用来控制 XML 序列化的属性](attributes-that-control-xml-serialization.md)
- [如何：指定 XML 流的替代元素名称](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
