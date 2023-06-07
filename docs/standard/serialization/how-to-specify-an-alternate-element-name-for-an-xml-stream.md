---
title: 如何：指定 XML 流的替代元素名称
description: 了解如何使用替代元素名称创建 XML 流，例如，对于需要大体相同信息（略有差异）的 XML Web service。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML serialization, overriding
- serialization, overriding
- XML stream, specifying alternate element name for
- overriding XML serialization
- classes, overriding
- overriding classes
ms.assetid: 5cc1c0b0-f94b-4525-9a41-88a582cd6668
ms.openlocfilehash: d7e482ee6e1e1a7318ab05766508537d4b87789e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289585"
---
# <a name="how-to-specify-an-alternate-element-name-for-an-xml-stream"></a>如何：指定 XML 流的替代元素名称
  
使用 <xref:System.Xml.Serialization.XmlSerializer>，可以用同一组类生成多个 XML 流。 由于两个不同的 XML Web services 需要的基本信息相同（略有差异），因此您或许希望用同一组类生成多个 XML 流。 例如，假设有两个处理书籍订单的 XML Web services，它们都需要 ISBN 号。 一个服务使用标记 \<ISBN>，而另一个使用标记 \<BookID>。 您已经有一个名为 `Book` 的类，其中包含名为 `ISBN` 的字段。 当序列化 `Book` 类的实例时，该实例将在默认情况下使用成员名称 (ISBN) 作为标记元素名称。 对于第一个 XML Web services，以上行为与预期相同。 但如果要将 XML 流发送至第二个 XML Web services，则必须重写序列化，以便使标记的元素名称采用 `BookID`。  
  
## <a name="to-create-an-xml-stream-with-an-alternate-element-name"></a>用替代元素名称创建 XML 流  
  
1. 创建 <xref:System.Xml.Serialization.XmlElementAttribute> 类的一个实例。  
  
2. 将 <xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> 的 <xref:System.Xml.Serialization.XmlElementAttribute> 设置为“BookID”。  
  
3. 创建 <xref:System.Xml.Serialization.XmlAttributes> 类的一个实例。  
  
4. 向通过 `XmlElementAttribute` 的 <xref:System.Xml.Serialization.XmlAttributes.XmlElements%2A> 属性访问的集合中添加 <xref:System.Xml.Serialization.XmlAttributes> 对象。  
  
5. 创建 <xref:System.Xml.Serialization.XmlAttributeOverrides> 类的一个实例。  
  
6. 将 `XmlAttributes` 添加至 <xref:System.Xml.Serialization.XmlAttributeOverrides>，同时传递要重写的对象类型以及要被重写的成员名称。  
  
7. 用 `XmlSerializer` 创建 `XmlAttributeOverrides` 类的实例。  
  
8. 创建 `Book` 类的实例，并将其序列化或反序列化。  
  
## <a name="example"></a>示例  
  
```vb  
Public Function SerializeOverride()  
    ' Creates an XmlElementAttribute with the alternate name.  
    Dim myElementAttribute As XmlElementAttribute = _  
    New XmlElementAttribute()  
    myElementAttribute.ElementName = "BookID"  
    Dim myAttributes As XmlAttributes = New XmlAttributes()  
    myAttributes.XmlElements.Add(myElementAttribute)  
    Dim myOverrides As XmlAttributeOverrides = New XmlAttributeOverrides()  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes)  
    Dim mySerializer As XmlSerializer = _  
    New XmlSerializer(GetType(Book), myOverrides)  
    Dim b As Book = New Book()  
    b.ISBN = "123456789"  
    ' Creates a StreamWriter to write the XML stream to.  
    Dim writer As StreamWriter = New StreamWriter("Book.xml")  
    mySerializer.Serialize(writer, b);  
End Class  
```  
  
```csharp  
public void SerializeOverride()  
{  
    // Creates an XmlElementAttribute with the alternate name.  
    XmlElementAttribute myElementAttribute = new XmlElementAttribute();  
    myElementAttribute.ElementName = "BookID";  
    XmlAttributes myAttributes = new XmlAttributes();  
    myAttributes.XmlElements.Add(myElementAttribute);  
    XmlAttributeOverrides myOverrides = new XmlAttributeOverrides();  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes);  
    XmlSerializer mySerializer =
    new XmlSerializer(typeof(Book), myOverrides)  
    Book b = new Book();  
    b.ISBN = "123456789"  
    // Creates a StreamWriter to write the XML stream to.  
    StreamWriter writer = new StreamWriter("Book.xml");  
    mySerializer.Serialize(writer, b);  
}  
```  
  
 XML 流可能如下所示。  
  
```xml  
<Book>  
    <BookID>123456789</BookID>  
</Book>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Serialization.XmlElementAttribute>
- <xref:System.Xml.Serialization.XmlAttributes>
- <xref:System.Xml.Serialization.XmlAttributeOverrides>
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
