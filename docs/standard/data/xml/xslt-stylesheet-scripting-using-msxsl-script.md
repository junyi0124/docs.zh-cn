---
title: 使用 <msxsl:script> 编写 XSLT 样式表脚本
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 60e2541b-0cea-4b2e-a4fa-85f4c50f1bef
ms.openlocfilehash: 206a659656f1019af1540b9b2476ae7fe9ba93eb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685105"
---
# <a name="xslt-stylesheet-scripting-using-msxslscript"></a>使用 \<msxsl:script> 编写 XSLT 样式表脚本

<xref:System.Xml.Xsl.XslTransform> 类使用 `script` 元素支持嵌入的脚本。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。  
  
 <xref:System.Xml.Xsl.XslTransform> 类使用 `script` 元素支持嵌入的脚本。 加载样式表时，任何已定义的函数都会通过包装在类定义中来编译为 Microsoft 中间语言 (MSIL)，因此不会有任何性能损失。  
  
 `<msxsl:script>` 元素定义如下：  
  
```xml  
<msxsl:script language = "language-name" implements-prefix = "prefix of user namespace"> </msxsl:script>  
```  
  
 其中 `msxsl` 是绑定到命名空间 `urn:schemas-microsoft-com:xslt` 的前缀。  
  
 `language` 属性不是强制属性，但如果指定该属性，其值必须是下列值之一：`C#`、`VB`、`JScript`、`JavaScript`、`VisualBasic` 或 `CSharp`。 如果未指定，则默认语言为 JScript。 `language-name` 不区分大小写，所以“JavaScript”和“javascript”等效。  
  
 `implements-prefix` 属性是必选项。 此属性用于声明命名空间并将其与脚本块关联。 此属性的值是表示命名空间的前缀。 此命名空间可以在样式表中的某一位置定义。  
  
 因为 `msxsl:script` 元素属于命名空间 `urn:schemas-microsoft-com:xslt`，因此样式表必须包含命名空间声明 `xmlns:msxsl=urn:schemas-microsoft-com:xslt`。  
  
 如果脚本调用方没有 <xref:System.Security.Permissions.SecurityPermissionFlag> 访问权限，样式表中的脚本绝不会编译，且无法调用 <xref:System.Xml.Xsl.XslTransform.Load%2A>。  
  
 如果调用方有 `UnmanagedCode` 权限，则脚本将编译，但允许的操作取决于加载时提供的证据。  
  
 如果使用一个接受 <xref:System.Xml.Xsl.XslTransform.Load%2A> 或 <xref:System.Xml.XmlReader> 的 <xref:System.Xml.XPath.XPathNavigator> 方法加载样式表，则需要使用接受 <xref:System.Xml.Xsl.XslTransform.Load%2A> 自变量作为其参数的 <xref:System.Security.Policy.Evidence> 重载。 为提供证据，调用方必须拥有 <xref:System.Security.Permissions.SecurityPermissionFlag> 权限，以提供脚本程序集的 `Evidence`。 如果调用方没有此权限，则可以将 `Evidence` 参数设置为 `null`。 这会导致 <xref:System.Xml.Xsl.XslTransform.Load%2A> 函数在发现脚本时失败。 `ControlEvidence` 权限是一种权力很大的权限，只应授予高度可信任的代码。  
  
 若要从您的程序集中得到证据，请使用 `this.GetType().Assembly.Evidence`。 若要从统一资源标识符 (URI) 得到证据，请使用 `Evidence e = XmlSecureResolver.CreateEvidenceForUrl(stylesheetURI)`。  
  
 如果使用接受 <xref:System.Xml.Xsl.XslTransform.Load%2A> 但没有 <xref:System.Xml.XmlResolver> 的 `Evidence` 方法，则程序集的安全区域默认为“完全信任”。 有关详细信息，请参阅 <xref:System.Security.SecurityZone> 和[命名权限集](/previous-versions/dotnet/netframework-4.0/4652tyx7(v=vs.100))。  
  
 函数可以在 `msxsl:script` 元素内声明。 下表显示了默认情况下支持的命名空间。 可以在列出的命名空间的外部使用类。 然而，这些类必须是完全限定的。  
  
|默认命名空间|描述|  
|------------------------|-----------------|  
|System|系统类。|  
|System.Collection|集合类。|  
|System.Text|文本类。|  
|System.Text.RegularExpressions|正则表达式类。|  
|System.Xml|核心 XML 类。|  
|System.Xml.Xsl|XSLT 类。|  
|System.Xml.XPath|XML 路径语言 (XPath) 类。|  
|Microsoft.VisualBasic|Microsoft Visual Basic 脚本类。|  
  
 声明函数时，该函数包含在脚本块中。 样式表可以包含多个脚本块，每个脚本块彼此独立运行。 也就是说，如果在脚本块的内部执行，则无法调用在其他脚本块中定义的函数，除非该脚本块声明为具有同一命名空间和同一脚本语言。 由于每个脚本块都可以使用自己的语言，因此脚本块的分析将遵照语言分析器的语法规则进行，必须使用适合所使用语言的语法。 例如，如果使用的是 C# 脚本块，则在该块中使用 XML 注释节点 `<!-- an XML comment -->` 是错误的。  
  
 由脚本函数定义的所提供的参数以及返回值必须是万维网联合会 (W3C) XPath 或 XSLT 类型之一。 下表展示了相应的 W3C 类型、相当的 .NET Framework 类（类型），以及 W3C 类型是 XPath 类型还是 XSLT 类型。  
  
|类型|相当的 .NET Framework 类（类型）|XPath 类型还是 XSLT 类型|  
|----------|----------------------------------------------|-----------------------------|  
|String|System.String|XPath|  
|Boolean|System.Boolean|XPath|  
|数字|System.Double|XPath|  
|Result Tree Fragment|System.Xml.XPath.XPathNavigator|XSLT|  
|Node Set|System.Xml.XPath.XPathNodeIterator|XPath|  
  
 如果脚本函数使用下列数值类型之一：Int16、UInt16、Int32、UInt32、Int64、UInt64、单数或小数，则会被强制转换为 Double，映射为 W3C XPath 类型的数字。 所有其他类型都通过调用 `ToString` 方法强制转换为字符串。  
  
 如果脚本函数使用上述类型以外的类型，或者如果函数在样式表加载到 <xref:System.Xml.Xsl.XslTransform> 对象中时不进行编译，则会引发异常。  
  
 使用 `msxsl:script` 元素时，强烈建议将脚本添加到 CDATA 部分中，无论使用何种语言。 例如，下面的 XML 显示放置代码的 CDATA 节的模板。  
  
```xml  
<msxsl:script implements-prefix='yourprefix' language='CSharp'>  
    <![CDATA[  
    ... your code here ...  
    ]]>  
</msxsl:script>  
```  
  
 强烈建议将所有脚本内容都放置在 CDATA 节内，因为给定语言的运算符、标识符或分隔符有可能被错误地解释为 XML。 下面的示例显示如何在脚本中使用逻辑 AND 运算符。  
  
```xml  
<msxsl:script implements-prefix='yourprefix' language='CSharp'>  
    public string book(string abc, string xyz)  
    {  
        if ((abc == bar) && (abc == xyz)) return bar + xyz;  
        else return null;  
    }  
</msxsl:script>  
```  
  
 由于“&amp;”符没有转义，因此将引发异常。 文档以 XML 形式加载，并且不对 `msxsl:script` 元素标记之间的文本应用任何特殊处理。  
  
## <a name="example"></a>示例  

 已知圆的半径，下面的示例使用嵌入脚本计算圆的周长。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public Class Sample  
  
   Private Const filename As String = "number.xml"  
   Private Const stylesheet As String = "calc.xsl"  
  
   Public Shared Sub Main()  
  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XmlTextWriter to output to the console.
    Dim writer As XmlTextWriter = New XmlTextWriter(Console.Out)  
    writer.Formatting = Formatting.Indented  
  
    'Transform the file.  
    xslt.Transform(doc, Nothing, writer, Nothing)  
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
   private const String filename = "number.xml";  
   private const String stylesheet = "calc.xsl";  
  
   public static void Main()  
   {  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XmlTextWriter to output to the console.
    XmlTextWriter writer = new XmlTextWriter(Console.Out);  
    writer.Formatting = Formatting.Indented;  
  
    //Transform the file.  
    xslt.Transform(doc, null, writer, null);  
    writer.Close();  
  }  
}  
```  
  
## <a name="input"></a>输入  

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
  
 calc.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
    xmlns:msxsl="urn:schemas-microsoft-com:xslt"  
    xmlns:user="urn:my-scripts">  
  
  <msxsl:script language="C#" implements-prefix="user">  
     <![CDATA[  
     public double circumference(double radius)  
     {  
       double pi = 3.14;  
       double circ = pi*radius*2;  
       return circ;  
     }  
      ]]>  
   </msxsl:script>  
  
  <xsl:template match="data">
  <circles>  
  
  <xsl:for-each select="circle">  
    <circle>  
    <xsl:copy-of select="node()"/>  
       <circumference>  
          <xsl:value-of select="user:circumference(radius)"/>
       </circumference>  
    </circle>  
  </xsl:for-each>  
  </circles>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
## <a name="output"></a>Output  
  
```xml  
<circles xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">  
  <circle>  
    <radius>12</radius>  
    <circumference>75.36</circumference>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
    <circumference>235.5</circumference>  
  </circle>  
</circles>
```  
  
## <a name="see-also"></a>请参阅

- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
