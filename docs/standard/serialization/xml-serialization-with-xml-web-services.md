---
title: 使用 XML Web services 进行 XML 序列化
description: 了解在 XML Web services 体系结构中用作传输机制的 XML 序列化。 XML 序列化由 XmlSerializer 类执行。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML Web services, XML serialization
- XML serialization, XML Web services
- SOAP, XML serialization
- asmx files
- serialization, SOAP
- XML serialization, attributes
- attributes [.NET], XML serialization
- .asmx files
- encoded XML serialization
- literal XML serialization
- serialization, attributes
ms.assetid: a416192f-8102-458e-bc0a-0b8f3f784da9
ms.openlocfilehash: 64b5911ca110ae4ef08f9003898bb817d8b8dd79
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676538"
---
# <a name="xml-serialization-with-xml-web-services"></a>使用 XML Web services 进行 XML 序列化

XML 序列化是在 XML Web services 体系结构中使用的基础传输机制，由 <xref:System.Xml.Serialization.XmlSerializer> 类执行。 要控制由 XML Web service 生成的 XML，可以对用于创建 XML Web service (.asmx) 的文件的类、返回值、参数和字段应用 [控制 XML 序列化的特性](attributes-that-control-xml-serialization.md)和 [控制编码的 SOAP 序列化的特性](attributes-that-control-encoded-soap-serialization.md)中列出的特性。 有关创建 XML Web service 的更多信息，请参阅[使用 ASP.NET 的 XML Web service](/previous-versions/dotnet/netframework-4.0/ba0z6a33(v=vs.100))。  
  
## <a name="literal-and-encoded-styles"></a>文本样式和编码样式  

 XML Web service 生成的 XML 可以用两种方式设置格式，一种是文本方式，另一种是编码方式，如[自定义 SOAP 消息格式设置](/previous-versions/dotnet/netframework-4.0/dkwy2d72(v=vs.100))中所述。 因此有两组控制 XML 序列化的属性。 [控制 XML 序列化的特性](attributes-that-control-xml-serialization.md)中列出的属性旨在控制文本样式的 XML。 [控制编码的 SOAP 序列化的特性](attributes-that-control-encoded-soap-serialization.md)中列出的特性控制编码样式。 通过有选择地应用这些属性，可以调整应用程序，使其返回两种样式或其中一种。 而且，这些特性可以根据需要应用于返回值和参数。  
  
### <a name="example-of-using-both-styles"></a>使用两种样式的示例  

 创建 XML Web services 时，可以对方法使用两组特性。 在下面的代码示例中，名为 `MyService` 的类包含两种 XML Web services 方法：`MyLiteralMethod` 和 `MyEncodedMethod`。 这两种方法执行相同的功能，即返回 `Order` 类的实例。 在 `Order` 类中，<xref:System.Xml.Serialization.XmlTypeAttribute> 和 <xref:System.Xml.Serialization.SoapTypeAttribute> 特性都应用于 `OrderID` 字段，且这两个特性的 `ElementName` 属性设置为不同的值。  
  
 要运行此示例，请将代码粘贴到扩展名为 .asmx 的文件中，然后将该文件放入由 Internet 信息服务 (IIS) 管理的虚拟目录中。 在 HTML 浏览器（如 Internet Explorer）中键入计算机、虚拟目录和文件的名称。  
  
```vb  
<%@ WebService Language="VB" Class="MyService" %>  
Imports System  
Imports System.Web.Services  
Imports System.Web.Services.Protocols  
Imports System.Xml.Serialization  
Public Class Order  
    ' Both types of attributes can be applied. Depending on which type  
    ' the method used, either one will affect the call.  
    <SoapElement(ElementName:= "EncodedOrderID"), _  
    XmlElement(ElementName:= "LiteralOrderID")> _  
    public OrderID As String  
End Class  
  
Public Class MyService  
    <WebMethod, SoapDocumentMethod> _  
    public Function MyLiteralMethod() As Order
        Dim myOrder As Order = New Order()  
        return myOrder  
    End Function  
    <WebMethod, SoapRpcMethod> _  
    public Function MyEncodedMethod() As Order
        Dim myOrder As Order = New Order()  
        return myOrder  
    End Function  
End Class  
```  
  
```csharp  
<%@ WebService Language="C#" Class="MyService" %>  
using System;  
using System.Web.Services;  
using System.Web.Services.Protocols;  
using System.Xml.Serialization;  
public class Order {  
    // Both types of attributes can be applied. Depending on which type  
    // the method used, either one will affect the call.  
    [SoapElement(ElementName = "EncodedOrderID")]  
    [XmlElement(ElementName = "LiteralOrderID")]  
    public String OrderID;  
}  
public class MyService {  
    [WebMethod][SoapDocumentMethod]  
    public Order MyLiteralMethod(){  
        Order myOrder = new Order();  
        return myOrder;  
    }  
    [WebMethod][SoapRpcMethod]  
    public Order MyEncodedMethod(){  
        Order myOrder = new Order();  
        return myOrder;  
    }  
}  
```  
  
 下面的代码示例调用 `MyLiteralMethod`。 会将元素名称更改为“LiteralOrderID”。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethodResponse xmlns="http://tempuri.org/">  
            <MyLiteralMethodResult>  
                <LiteralOrderID>string</LiteralOrderID>  
            </MyLiteralMethodResult>  
        </MyLiteralMethodResponse>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 下面的代码示例调用 `MyEncodedMethod`。 元素名称为“EncodedOrderID”。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/" xmlns:tns="http://tempuri.org/" xmlns:types="http://tempuri.org/encodedTypes" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body soap:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">  
        <tns:MyEncodedMethodResponse>  
            <MyEncodedMethodResult href="#id1" />  
        </tns:MyEncodedMethodResponse>  
        <types:Order id="id1" xsi:type="types:Order">  
            <EncodedOrderID xsi:type="xsd:string">string</EncodedOrderID>  
        </types:Order>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="applying-attributes-to-return-values"></a>将特性应用于返回值  

 您还可以将特性应用于返回值，以控制命名空间和元素名称等。 下面的代码示例将 `XmlElementAttribute` 特性应用于 `MyLiteralMethod` 方法的返回值。 这样可让您控制命名空间和元素名称。  
  
```vb  
<WebMethod, SoapDocumentMethod> _  
public Function MyLiteralMethod() As _  
<XmlElement(Namespace:="http://www.cohowinery.com", _  
ElementName:= "BookOrder")> _  
Order
    Dim myOrder As Order = New Order()  
    return myOrder  
End Function  
```  
  
```csharp  
[return: XmlElement(Namespace = "http://www.cohowinery.com",  
ElementName = "BookOrder")]  
[WebMethod][SoapDocumentMethod]  
public Order MyLiteralMethod(){  
    Order myOrder = new Order();  
    return myOrder;  
}  
```  
  
 代码被调用时，将返回类似如下所示的 XML。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethodResponse xmlns="http://tempuri.org/">  
            <BookOrder xmlns="http://www.cohowinery.com">  
                <LiteralOrderID>string</LiteralOrderID>  
            </BookOrder>  
        </MyLiteralMethodResponse>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="attributes-applied-to-parameters"></a>应用于参数的特性  

 您还可以将属性应用于参数，以指定命名空间和元素名称等。 下面的代码示例向 `MyLiteralMethodResponse` 方法中添加一个参数，并将 `XmlAttributeAttribute` 特性应用于该参数。 为该参数设置了元素名称和命名空间。  
  
```vb  
<WebMethod, SoapDocumentMethod> _  
public Function MyLiteralMethod(<XmlElement _  
("MyOrderID", Namespace:="http://www.microsoft.com")>ID As String) As _  
<XmlElement(Namespace:="http://www.cohowinery.com", _  
ElementName:= "BookOrder")> _  
Order
    Dim myOrder As Order = New Order()  
    myOrder.OrderID = ID  
    return myOrder  
End Function  
```  
  
```csharp  
[return: XmlElement(Namespace = "http://www.cohowinery.com",  
ElementName = "BookOrder")]  
[WebMethod][SoapDocumentMethod]  
public Order MyLiteralMethod([XmlElement("MyOrderID",
Namespace="http://www.microsoft.com")] string ID){  
    Order myOrder = new Order();  
    myOrder.OrderID = ID;  
    return myOrder;  
}
```  
  
 SOAP 请求会类似于：  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethod xmlns="http://tempuri.org/">  
            <MyOrderID xmlns="http://www.microsoft.com">string</MyOrderID>  
        </MyLiteralMethod>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="applying-attributes-to-classes"></a>将特性应用于类  

 如果需要控制与类相关联的元素的命名空间，则可以根据需要应用 `XmlTypeAttribute`、`XmlRootAttribute` 和 `SoapTypeAttribute`。 下面的代码示例将这三者全部应用于 `Order` 类。  
  
```vb  
<XmlType("BigBookService"), _  
SoapType("SoapBookService"), _  
XmlRoot("BookOrderForm")> _  
Public Class Order  
    ' Both types of attributes can be applied. Depending on which  
    ' the method used, either one will affect the call.  
    <SoapElement(ElementName:= "EncodedOrderID"), _  
    XmlElement(ElementName:= "LiteralOrderID")> _  
    public OrderID As String  
End Class  
```  
  
```csharp  
[XmlType("BigBooksService", Namespace = "http://www.cpandl.com")]  
[SoapType("SoapBookService")]  
[XmlRoot("BookOrderForm")]  
public class Order {  
    // Both types of attributes can be applied. Depending on which  
    // the method used, either one will affect the call.  
    [SoapElement(ElementName = "EncodedOrderID")]  
    [XmlElement(ElementName = "LiteralOrderID")]  
    public String OrderID;  
}  
```  
  
 检查服务说明时，可以看见 `XmlTypeAttribute` 和 `SoapTypeAttribute` 的应用结果，如下面的代码示例所示。  
  
```xml
<s:element name="BookOrderForm" type="s0:BigBookService" />
<s:complexType name="BigBookService">
  <s:sequence>
    <s:element minOccurs="0" maxOccurs="1" name="LiteralOrderID" type="s:string" />
  </s:sequence>

  <s:schema targetNamespace="http://tempuri.org/encodedTypes">
    <s:complexType name="SoapBookService">
      <s:sequence>
        <s:element minOccurs="1" maxOccurs="1" name="EncodedOrderID" type="s:string" />
      </s:sequence>
    </s:complexType>
  </s:schema>
</s:complexType>
```
  
 `XmlRootAttribute` 的作用也可以在 HTTP GET 和 HTTP POST 结果中看见，如下所示。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<BookOrderForm xmlns="http://tempuri.org/">  
    <LiteralOrderID>string</LiteralOrderID>  
</BookOrderForm>  
```  
  
## <a name="see-also"></a>请参阅

- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- [用来控制编码的 SOAP 序列化的属性](attributes-that-control-encoded-soap-serialization.md)
- [如何：将对象序列化为 SOAP 编码的 XML 流](how-to-serialize-an-object-as-a-soap-encoded-xml-stream.md)
- [如何：替代编码的 SOAP XML 序列化](how-to-override-encoded-soap-xml-serialization.md)
- [XML 序列化简介](introducing-xml-serialization.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
