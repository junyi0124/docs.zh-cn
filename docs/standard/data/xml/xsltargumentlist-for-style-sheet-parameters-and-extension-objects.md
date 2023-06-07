---
title: 样式表参数和扩展对象的 XsltArgumentList
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: de2f0dce-6b98-4908-bba7-ed150cc50355
ms.openlocfilehash: fe227a2d3efc5c36b818b7f4431896e6f62b1f26
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685040"
---
# <a name="xsltargumentlist-for-style-sheet-parameters-and-extension-objects"></a>样式表参数和扩展对象的 XsltArgumentList

<xref:System.Xml.Xsl.XsltArgumentList> 类包含可扩展样式表语言转换 (XSLT) 参数和 XSLT 扩展对象。 传入 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法后，这些参数和扩展对象可以从样式表中进行调用。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 和 <xref:System.Xml.Xsl.XsltArgumentList> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行 XSLT 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。  
  
 <xref:System.Xml.Xsl.XsltArgumentList> 类包含 XSLT 参数和 XSLT 扩展对象。 传入 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法后，这些参数和扩展对象可以从样式表中进行调用。  
  
 与使用嵌入脚本相比，传递对象具有以下优点：  
  
- 改善了类的包装和重用。  
  
- 使样式表可以更小而且更容易维护。  
  
- 支持对所属命名空间未在支持的 <xref:System> 命名空间集中定义的类调用方法。  
  
- 支持使用 <xref:System.Xml.XPath.XPathNodeIterator> 将结果树片段传递到样式表。  
  
## <a name="xslt-style-sheet-parameters"></a>XSLT 样式表参数  

 使用 <xref:System.Xml.Xsl.XsltArgumentList> 方法将 XSLT 参数添加到 <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A>。 此时，限定名和命名空间统一资源标识符 (URI) 与参数对象关联。  
  
 参数对象应对应于某个万维网联合会 (W3C) 类型。 下表显示了相应的 W3C 类型、等效的 .NET Framework 类（类型），以及 W3C 类型是 XML 路径语言 (XPath) 类型还是 XSLT 类型。  
  
|W3C 类型|相当的 .NET Framework 类（类型）|XPath 类型还是 XSLT 类型|  
|--------------|----------------------------------------------|-----------------------------|  
|String|System.String|XPath|  
|Boolean|System.Boolean|XPath|  
|数字|System.Double|XPath|  
|Result Tree Fragment|System.Xml.XPath.XPathNavigator|XSLT|  
|Node Set|System.Xml.XPath.XPathNodeIterator|XPath|  
  
 如果参数对象不属于上述类，将根据需要被强制指定为 Double 或 String（以适用的为准）。 Int16、UInt16、Int32、UInt32、Int64、UInt64、Single 和 Decimal 类型被强制指定为 Double。 所有其他类型均被用 `ToString` 方法强制指定为 String。  
  
#### <a name="to-use-the-xslt-parameter-the-user-needs-to-do-the-following"></a>要使用 XSLT 参数，用户需要执行以下操作：  
  
1. 创建 <xref:System.Xml.Xsl.XsltArgumentList> 并使用 <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> 添加对象。  
  
2. 从样式表调用参数。  
  
3. 将 <xref:System.Xml.Xsl.XsltArgumentList> 传递到 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法。  
  
### <a name="example"></a>示例  

 下面的示例使用 <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> 方法创建一个参数来保存计算的折扣日期。 折扣日期计算为从订单日期算起的 20 天时间。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public class Sample  
  
   Private Const filename As String = "order.xml"  
   Private Const stylesheet As String = "discount.xsl"  
  
   Public Shared Sub Main()  
  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XsltArgumentList.  
    Dim xslArg As XsltArgumentList = New XsltArgumentList  
  
    'Calculate the discount date.  
    Dim today As DateTime = DateTime.Now  
    Dim d As DateTime = today.AddDays(20)  
    xslArg.AddParam("discount", "", d.ToString())  
  
    'Create an XmlTextWriter to handle the output.  
    Dim writer As XmlTextWriter = New XmlTextWriter("orderout.xml", Nothing)  
  
    'Transform the file.  
    xslt.Transform(doc, xslArg, writer, Nothing)  
  
    writer.Close()  
  
  End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "order.xml";  
   private const String stylesheet = "discount.xsl";  
  
   public static void Main() {  
  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XsltArgumentList.  
    XsltArgumentList xslArg = new XsltArgumentList();  
  
    //Calculate the discount date.  
    DateTime today = DateTime.Now;  
    DateTime d = today.AddDays(20);  
    xslArg.AddParam("discount", "", d.ToString());  
  
    //Create an XmlTextWriter to handle the output.  
    XmlTextWriter writer = new XmlTextWriter("orderout.xml", null);  
  
    //Transform the file.  
    xslt.Transform(doc, xslArg, writer, null);  
    writer.Close();  
  }  
}  
```  
  
### <a name="input"></a>输入  

 order.xml  
  
```xml  
<!--Represents a customer order-->  
<order>  
  <book ISBN='10-861003-324'>  
    <title>The Handmaid's Tale</title>  
    <price>19.95</price>  
  </book>  
  <cd ISBN='2-3631-4'>  
    <title>Americana</title>  
    <price>16.95</price>  
  </cd>  
</order>  
```  
  
 discount.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
  <xsl:param name="discount"/>  
  <xsl:template match="/">  
    <order>  
      <xsl:variable name="sub-total" select="sum(//price)"/>  
      <total><xsl:value-of select="$sub-total"/></total>  
      15% discount if paid by: <xsl:value-of select="$discount"/>  
    </order>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
### <a name="output"></a>Output  
  
```xml  
<order>  
   <total>36.9</total>
   15% discount if paid by: 5/6/2001 5:01:15 PM
</order>  
```  
  
## <a name="xslt-extension-objects"></a>XSLT 扩展对象  

 使用 <xref:System.Xml.Xsl.XsltArgumentList> 方法将 XSLT 扩展对象添加到 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A>。 此时，限定名和命名空间 URI 与扩展对象关联。  
  
 添加对象时，<xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 的调用方在安全策略中必须是完全受信任的。 如果调用方不是完全受信任的，则添加操作将失败。  
  
 尽管对象已成功添加，但不能保证执行将成功。 调用 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法时，将针对在 <xref:System.Xml.Xsl.XslTransform.Load%2A> 时提供的证据来计算权限，并将权限集分配给整个转换过程。 如果一个扩展对象尝试启动一个操作，而该操作需要权限集中所没有的权限，就会引发异常。  
  
 从扩展对象返回的数据类型是四种 XPath 基本数据类型之一：数字、字符串、布尔值或节点集。  
  
#### <a name="to-use-the-xslt-extension-object-the-user-needs-to-do-the-following"></a>要使用 XSLT 扩展对象，用户需要执行以下操作：  
  
1. 创建 <xref:System.Xml.Xsl.XsltArgumentList> 并使用 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 添加扩展对象。  
  
2. 从样式表调用扩展对象。  
  
3. 将 <xref:System.Xml.Xsl.XsltArgumentList> 传递到 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法。  
  
### <a name="example"></a>示例  

 已知圆的半径，下面的示例计算圆的周长。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public Class Sample  
   Private Const filename As String = "number.xml"  
   Private Const stylesheet As String = "circle.xsl"  
  
   Public Shared Sub Main()  
        Dim test As Sample = New Sample  
   End Sub  
  
  Public Sub New()  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XsltArgumentList.  
    Dim xslArg As XsltArgumentList = New XsltArgumentList  
  
    'Add an object to calculate the circumference of the circle.  
    Dim obj As Calculate = New Calculate  
    xslArg.AddExtensionObject("urn:myObj", obj)  
  
    'Create an XmlTextWriter to output to the console.  
    Dim writer As XmlTextWriter = New XmlTextWriter(Console.Out)  
  
    'Transform the file.  
    xslt.Transform(doc, xslArg, writer, Nothing)  
    writer.Close()  
  
  End Sub  
  
  'Calculates the circumference of a circle given the radius.  
  Public Class Calculate  
  
    Private circ As double = 0  
  
    Public Function Circumference(radius As Double) As Double  
       circ = Math.PI*2*radius  
       Return circ  
    End Function  
  End Class  
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "number.xml";  
   private const String stylesheet = "circle.xsl";  
  
   public static void Main() {  
  
        Sample test = new Sample();  
    }  
  
  public Sample() {  
  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XsltArgumentList.  
    XsltArgumentList xslArg = new XsltArgumentList();  
  
    //Add an object to calculate the circumference of the circle.  
    Calculate obj = new Calculate();  
    xslArg.AddExtensionObject("urn:myObj", obj);  
  
    //Create an XmlTextWriter to output to the console.  
    XmlTextWriter writer = new XmlTextWriter(Console.Out);  
  
    //Transform the file.  
    xslt.Transform(doc, xslArg, writer, null);  
    writer.Close();  
  
  }  
  
  //Calculates the circumference of a circle given the radius.  
  public class Calculate {  
  
    private double circ = 0;  
  
    public double Circumference(double radius){  
       circ = Math.PI*2*radius;  
       return circ;  
    }  
  }  
}  
```  
  
### <a name="input"></a>输入  

 number.xml  
  
```xml  
<?xml version='1.0'?>  
<data>  
  <circle>  
    <radius>12</radius>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
  </circle>  
</data>
```  
  
 circle.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
    xmlns:myObj="urn:myObj">  
  
  <xsl:template match="data">  
  <circles>  
  <xsl:for-each select="circle">  
    <circle>  
    <xsl:copy-of select="node()"/>  
       <circumference>  
          <xsl:value-of select="myObj:Circumference(radius)"/>
       </circumference>  
    </circle>  
  </xsl:for-each>  
  </circles>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
### <a name="output"></a>Output  

 `<circles xmlns:myObj="urn:myObj">`  
  
 `<circle>`  
  
 `<radius>12</radius>`  
  
 `<circumference>75.398223686155</circumference>`  
  
 `</circle>`  
  
 `<circle>`  
  
 `<radius>37.5</radius>`  
  
 `<circumference>235.61944901923448</circumference>`  
  
 `</circle>`  
  
 `</circles>`  
  
## <a name="see-also"></a>请参阅

- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
