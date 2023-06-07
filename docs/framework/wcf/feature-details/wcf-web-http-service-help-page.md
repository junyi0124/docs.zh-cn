---
title: WCF Web HTTP 服务帮助页
ms.date: 03/30/2017
ms.assetid: 63c7c695-44b6-4f31-bb9c-00f2763f525e
ms.openlocfilehash: fe6b22c6ebd637d3aa2bcd3744cb6b73e71c9482
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266930"
---
# <a name="wcf-web-http-service-help-page"></a>WCF Web HTTP 服务帮助页

[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]提供了 WCF WEB HTTP 服务的自动帮助页。 此帮助页列出了每个操作的说明、请求和响应格式以及架构。 默认情况下关闭此功能。 当用户浏览到 WCF WEB HTTP 服务并在 URL 的末尾追加 "/Help" 时，将显示如下所 `http://localhost:8000/Customers/Help` 示的帮助页。  
  
 ![具有 WCF REST 帮助页打开的浏览器。](./media/wcf-web-http-service-help-page/windows-communication-foundation-rest-help-page.gif)  
  
 用户随后可以单击帮助页中列出的任何方法，并且所显示的该操作的详细页中显示了有关该方法的更多信息，其中包括消息格式和示例响应。 下图是方法帮助页的一个示例。  
  
 ![具有 WCF REST 帮助页详细信息的浏览器 GetCustomers 方法打开。](./media/wcf-web-http-service-help-page/windows-communication-foundation-rest-help-page-detail.gif)  
  
## <a name="using-the-wcf-web-http-help-page"></a>使用 WCF Web HTTP 帮助页  

 WCF WEB HTTP 帮助页显示了每个操作的简单说明（假设你使用 <xref:System.ComponentModel.DescriptionAttribute> 指定了操作）。 此特性接受一个字符串，该字符串中包含它所应用到的操作的简短说明。 例如，下面的代码演示如何使用 <xref:System.ComponentModel.DescriptionAttribute> 来提供简短说明。  
  
```csharp
[OperationContract]  
[WebGet(UriTemplate="/template1", BodyStyle = WebMessageBodyStyle.Bare)]  
[Description("Description for GET /template1")]  
SyndicationFeedFormatter GetTemplate1();  
```  
  
 若要打开 WCF WEB HTTP 帮助页，你必须向服务的终结点添加一个终结点行为。 可以通过配置或代码完成此操作。 若要通过配置启用 WCF WEB HTTP 帮助页，请使用 `<webHttp>``enableHelp` 元素添加终结点行为，将 `true` 设置为 ，添加一个终结点并将其配置为使用终结点行为。 下面的配置代码演示如何执行此操作。  
  
```xml  
<endpointBehaviors>  
   <behavior name="RESTEndpointBehavior">  
      <webHttp enableHelp="true"/>  
   </behavior>  
</endpointBehaviors>  
<!-- ... -->  
<services>  
   <service behaviorConfiguration="RESTWebServiceBehavior" name="RESTWebService">      <endpoint address="" kind="webHttpEndpoint" behaviorConfiguration="RESTEndpointBehavior" contract="IHello" />  
  
      <!-- ... -->  
   </service>  
</services>  
```  
  
 若要通过代码启用 WCF Web HTTP 帮助页，请添加一个服务终结点，将 <xref:System.ServiceModel.Description.WebHttpBehavior> 添加到该终结点，并将 <xref:System.ServiceModel.Description.WebHttpBehavior.HelpEnabled%2A> 设置为 `true`。 下面的代码演示如何执行此操作。  
  
```csharp
using (WebServiceHost host = new WebServiceHost(typeof(Service), new Uri("http://localhost:8000/Customers")))  
{  
   host.AddServiceEndpoint(typeof(ICustomerCollection), new WebHttpBinding(), "");
   host.Description.Endpoints[0].Behaviors.Add(new WebHttpBehavior { EnableHelp = true });  
   // ...  
}  
```  
  
 帮助页基于 XHTML，它带有可标识页的不同部分的标记。 这样使客户端能够使用 <xref:System.Xml.Linq.XElement> 或其他 XLinq API 以编程方式访问帮助页。  
  
## <a name="schemas-used-in-the-wcf-web-http-service-help-page"></a>WCF Web HTTP 服务帮助页中使用的架构  

 WCF Web HTTP 服务帮助页中使用了以下架构。  
  
```xml  
<?xml version="1.0" encoding="utf-16"?>  
<xs:schema xmlns:tns="http://schemas.microsoft.com/2003/10/Serialization/" attributeFormDefault="qualified" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/2003/10/Serialization/" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:element name="anyType" nillable="true" type="xs:anyType" />  
  <xs:element name="anyURI" nillable="true" type="xs:anyURI" />  
  <xs:element name="base64Binary" nillable="true" type="xs:base64Binary" />  
  <xs:element name="boolean" nillable="true" type="xs:boolean" />  
  <xs:element name="byte" nillable="true" type="xs:byte" />  
  <xs:element name="dateTime" nillable="true" type="xs:dateTime" />  
  <xs:element name="decimal" nillable="true" type="xs:decimal" />  
  <xs:element name="double" nillable="true" type="xs:double" />  
  <xs:element name="float" nillable="true" type="xs:float" />  
  <xs:element name="int" nillable="true" type="xs:int" />  
  <xs:element name="long" nillable="true" type="xs:long" />  
  <xs:element name="QName" nillable="true" type="xs:QName" />  
  <xs:element name="short" nillable="true" type="xs:short" />  
  <xs:element name="string" nillable="true" type="xs:string" />  
  <xs:element name="unsignedByte" nillable="true" type="xs:unsignedByte" />  
  <xs:element name="unsignedInt" nillable="true" type="xs:unsignedInt" />  
  <xs:element name="unsignedLong" nillable="true" type="xs:unsignedLong" />  
  <xs:element name="unsignedShort" nillable="true" type="xs:unsignedShort" />  
  <xs:element name="char" nillable="true" type="tns:char" />  
  <xs:simpleType name="char">  
    <xs:restriction base="xs:int" />  
  </xs:simpleType>  
  <xs:element name="duration" nillable="true" type="tns:duration" />  
  <xs:simpleType name="duration">  
    <xs:restriction base="xs:duration">  
      <xs:pattern value="\-?P(\d*D)?(T(\d*H)?(\d*M)?(\d*(\.\d*)?S)?)?" />  
      <xs:minInclusive value="-P10675199DT2H48M5.4775808S" />  
      <xs:maxInclusive value="P10675199DT2H48M5.4775807S" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:element name="guid" nillable="true" type="tns:guid" />  
  <xs:simpleType name="guid">  
    <xs:restriction base="xs:string">  
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:attribute name="FactoryType" type="xs:QName" />  
  <xs:attribute name="Id" type="xs:ID" />  
  <xs:attribute name="Ref" type="xs:IDREF" />  
</xs:schema>  
  
<?xml version="1.0" encoding="utf-16"?>  
<xs:schema xmlns:tns="http://microsoft.com/wsdl/types/" elementFormDefault="qualified" targetNamespace="http://microsoft.com/wsdl/types/" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:simpleType name="guid">  
    <xs:restriction base="xs:string">  
      <xs:pattern value="[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:simpleType name="char">  
    <xs:restriction base="xs:unsignedShort" />  
  </xs:simpleType>  
</xs:schema>  
  
<?xml version="1.0" encoding="utf-16"?>  
<xs:schema xmlns:tns="http://schemas.datacontract.org/2004/07/System" elementFormDefault="qualified" targetNamespace="http://schemas.datacontract.org/2004/07/System" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />  
  <xs:complexType name="DateTimeOffset">  
    <xs:annotation>  
      <xs:appinfo>  
        <IsValueType xmlns="http://schemas.microsoft.com/2003/10/Serialization/">true</IsValueType>  
      </xs:appinfo>  
    </xs:annotation>  
    <xs:sequence>  
      <xs:element name="DateTime" type="xs:dateTime" />  
      <xs:element name="OffsetMinutes" type="xs:short" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="DateTimeOffset" nillable="true" type="tns:DateTimeOffset" />  
  <xs:complexType name="DBNull">  
    <xs:sequence />  
  </xs:complexType>  
  <xs:element name="DBNull" nillable="true" type="tns:DBNull" />  
</xs:schema>  
  
<?xml version="1.0" encoding="utf-16"?>  
<xs:schema xmlns:tns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />  
  <xs:complexType name="ArrayOfboolean">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="boolean" type="xs:boolean" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfboolean" nillable="true" type="tns:ArrayOfboolean" />  
  <xs:complexType name="ArrayOfchar">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="char" type="ser:char" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfchar" nillable="true" type="tns:ArrayOfchar" />  
  <xs:complexType name="ArrayOfdateTime">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="dateTime" type="xs:dateTime" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfdateTime" nillable="true" type="tns:ArrayOfdateTime" />  
  <xs:complexType name="ArrayOfdecimal">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="decimal" type="xs:decimal" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfdecimal" nillable="true" type="tns:ArrayOfdecimal" />  
  <xs:complexType name="ArrayOfdouble">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="double" type="xs:double" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfdouble" nillable="true" type="tns:ArrayOfdouble" />  
  <xs:complexType name="ArrayOffloat">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="float" type="xs:float" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOffloat" nillable="true" type="tns:ArrayOffloat" />  
  <xs:complexType name="ArrayOfguid">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="guid" type="ser:guid" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfguid" nillable="true" type="tns:ArrayOfguid" />  
  <xs:complexType name="ArrayOfint">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="int" type="xs:int" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfint" nillable="true" type="tns:ArrayOfint" />  
  <xs:complexType name="ArrayOflong">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="long" type="xs:long" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOflong" nillable="true" type="tns:ArrayOflong" />  
  <xs:complexType name="ArrayOfshort">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="short" type="xs:short" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfshort" nillable="true" type="tns:ArrayOfshort" />  
  <xs:complexType name="ArrayOfstring">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="string" nillable="true" type="xs:string" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfstring" nillable="true" type="tns:ArrayOfstring" />  
  <xs:complexType name="ArrayOfduration">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="duration" type="ser:duration" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfduration" nillable="true" type="tns:ArrayOfduration" />  
  <xs:complexType name="ArrayOfunsignedInt">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="unsignedInt" type="xs:unsignedInt" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfunsignedInt" nillable="true" type="tns:ArrayOfunsignedInt" />  
  <xs:complexType name="ArrayOfunsignedLong">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="unsignedLong" type="xs:unsignedLong" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfunsignedLong" nillable="true" type="tns:ArrayOfunsignedLong" />  
  <xs:complexType name="ArrayOfunsignedShort">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="unsignedShort" type="xs:unsignedShort" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfunsignedShort" nillable="true" type="tns:ArrayOfunsignedShort" />  
  <xs:complexType name="ArrayOfQName">  
    <xs:sequence>  
      <xs:element minOccurs="0" maxOccurs="unbounded" name="QName" nillable="true" type="xs:QName" />  
    </xs:sequence>  
  </xs:complexType>  
  <xs:element name="ArrayOfQName" nillable="true" type="tns:ArrayOfQName" />  
</xs:schema>  
```  
  
 有关数据协定序列化架构的详细信息，请参阅 [数据协定架构引用](data-contract-schema-reference.md)。
