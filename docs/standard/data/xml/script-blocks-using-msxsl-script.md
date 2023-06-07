---
title: 使用 msxsl:script 的脚本块
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fde6f43f-c594-486f-abcb-2211197fae20
ms.openlocfilehash: 1a2d1f0972bc610cb4943dacc74c1bae8c54012b
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032211"
---
# <a name="script-blocks-using-msxslscript"></a>使用 msxsl:script 的脚本块

> [!NOTE]
> 只有 .NET Framework 支持脚本块。 .NET Core 或 .NET 5.0（或更高版本）不支持。

<xref:System.Xml.Xsl.XslCompiledTransform> 类使用 `msxsl:script` 元素支持嵌入的脚本。 在加载样式表式，任何已定义的函数将通过代码文档对象模型 (CodeDOM) 编译为 Microsoft 中间语言 (MSIL) 并在运行时执行。 从嵌入的脚本块生成的程序集比为样式表生成的程序集独立。  
  
## <a name="enable-xslt-script"></a>启用 XSLT 脚本  

 支持嵌入式脚本是 <xref:System.Xml.Xsl.XslCompiledTransform> 类上可选的 XSLT 设置。 默认情况下禁用脚本支持。 要启用脚本支持，创建一个 <xref:System.Xml.Xsl.XsltSettings> 对象，将 <xref:System.Xml.Xsl.XsltSettings.EnableScript%2A> 属性设置为 `true`，然后将该对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 方法。  
  
> [!NOTE]
> 只有要求脚本支持并且处于完全可信的环境下时，才应启用 XSLT 脚本。  
  
## <a name="msxslscript-element-definition"></a>msxsl:script 元素定义  

 `msxsl:script` 元素是 Microsoft 对 XSLT 1.0 建议的扩展，包括以下定义：  
  
```xml  
<msxsl:script language = "language-name" implements-prefix = "prefix of user namespace"> </msxsl:script>  
```  
  
 `msxsl` 前缀绑定到 `urn:schemas-microsoft-com:xslt` 命名空间 URI 上。 样式表必须包括 `xmlns:msxsl=urn:schemas-microsoft-com:xslt` 命名空间声明。  
  
 `language` 属性是可选项。 属性值是嵌入式代码块的代码语言。 该语言使用 <xref:System.CodeDom.Compiler.CodeDomProvider.CreateProvider%2A?displayProperty=nameWithType> 方法映射到相应的 CodeDOM 编译器。 <xref:System.Xml.Xsl.XslCompiledTransform> 类可以支持任何 Microsoft .NET 语言，假定在计算机上已安装了相应的提供程序并且已在 machine.config 文件的 system.codedom 部分进行注册。 如果未指定 `language` 属性，则默认语言为 JScript。 语言名称不区分大小写，所以“JavaScript”和“javascript”等效。  
  
 `implements-prefix` 属性是必选项。 此属性用于声明命名空间并将其与脚本块关联。 此属性的值是表示命名空间的前缀。 此前缀可以在样式表中的某一位置定义。  
  
> [!NOTE]
> 当使用 `msxsl:script` 元素时，强烈建议无论使用何种语言，都应将脚本放置在 CDATA 节内。 因为脚本可以包含给定语言的运算符、标识符或分隔符，如果不包含在 CDATA 节中，可能会错误地作为 XML 解释。 以下 XML 显示可以放入代码的 CDATA 节的模板。  
  
```xml  
<msxsl:script implements-prefix='your-prefix' language='CSharp'>  
<![CDATA[  
// Code block.  
]]>  
</msxsl:script>  
```  
  
## <a name="script-functions"></a>脚本函数  

 函数可以在 `msxsl:script` 元素内声明。 声明函数时，该函数包含在脚本块中。 样式表可以包含多个脚本块，每个脚本块彼此独立运行。 也就是说，如果在脚本块的内部执行，则无法调用在其他脚本块中定义的函数，除非该脚本块声明为具有同一命名空间和同一脚本语言。 由于每个脚本块都可以使用自己的语言，因此脚本块的分析将遵照语言分析器的语法规则进行，我们建议您使用适合所使用语言的语法。 例如，如果在 Microsoft C# 脚本块中，请使用 C# 注释语法。  
  
 为函数提供的参数和返回值可以是任意类型。 因为 W3C XPath 类型是公共语言运行库 (CLR) 类型的子集，所以，对不属于 XPath 类型的类型进行类型转换。 下表显示相应的 W3C 类型和等效的 CLR 类型。  
  
|W3C 类型|CLR 类型|  
|--------------|--------------|  
|`String`|<xref:System.String>|  
|`Boolean`|<xref:System.Boolean>|  
|`Number`|<xref:System.Double>|  
|`Result Tree Fragment`|<xref:System.Xml.XPath.XPathNavigator>|  
|`Node Set`|<xref:System.Xml.XPath.XPathNodeIterator>|  
  
 CLR 数字类型转换为 <xref:System.Double>。 <xref:System.DateTime> 类型转换为 <xref:System.String>。 <xref:System.Xml.XPath.IXPathNavigable> 类型转换为 <xref:System.Xml.XPath.XPathNavigator>。 XPathNavigator[]  转换为 <xref:System.Xml.XPath.XPathNodeIterator>。  
  
 所有其他类型均将引发错误。  
  
### <a name="importing-namespaces-and-assemblies"></a>导入命名空间和程序集  

 <xref:System.Xml.Xsl.XslCompiledTransform> 类预定义了一组程序集和命名空间，默认情况下，通过 `msxsl:script` 元素支持这些程序集和命名空间。 但是，可以通过在 `msxsl:script` 块中导入程序集和命名空间，使用属于不在预定义列表中的某个命名空间的类和成员。  
  
#### <a name="assemblies"></a>程序集  

 默认情况下引用下列两个程序集：  
  
- System.dll  
  
- System.Xml.dll  
  
- Microsoft.VisualBasic.dll（如果脚本语言为 VB）  
  
 可以使用 `msxsl:assembly` 元素导入其他程序集。 包括在编译样式表时的程序集。 `msxsl:assembly` 元素具有以下定义：  
  
```xml  
<msxsl:script>  
  <msxsl:assembly name="system.assemblyName" />  
  <msxsl:assembly href="path-name" />  
    <![CDATA[  
    // User code  
    ]]>  
</msxsl:script>  
```  
  
 `name` 属性包含程序集的名称，`href` 属性包含程序集的路径。 程序集名称可以是全名，例如“System.Data, Version=2.0.3600.0, Culture=neutral, PublicKeyToken=b77a5c561934e089”，也可以是缩写名称，例如“System.Web”。  
  
#### <a name="namespaces"></a>命名空间  

 默认情况下包括下列命名空间：  
  
- System  
  
- System.Collection  
  
- System.Text  
  
- System.Text.RegularExpressions  
  
- System.Xml  
  
- System.Xml.Xsl  
  
- System.Xml.XPath  
  
- Microsoft.VisualBasic（如果脚本语言为 VB）  
  
 可以使用 `namespace` 属性添加对其他命名空间的支持。 属性值是命名空间的名称。  
  
```xml  
<msxsl:script>  
  <msxsl:using namespace="system.namespaceName" />  
    <![CDATA[  
    // User code  
    ]]>  
</msxsl:script>  
```  
  
## <a name="example"></a>示例  

 已知圆的半径，下面的示例使用嵌入脚本计算圆的周长。  
  
 [!code-csharp[XSLT_Script#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XSLT_Script/CS/xslt_script.cs#1)]
 [!code-vb[XSLT_Script#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XSLT_Script/VB/xslt_script.vb#1)]  
  
#### <a name="numberxml"></a>number.xml  

 [!code-xml[XSLT_Script#2](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Script/XML/number.xml#2)]  
  
#### <a name="calcxsl"></a>calc.xsl  

 [!code-xml[XSLT_Script#3](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Script/XML/calc.xsl#3)]  
  
### <a name="output"></a>Output  
  
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

- [XSLT 转换](xslt-transformations.md)
- [动态源代码生成和编译](../../../framework/reflection-and-codedom/dynamic-source-code-generation-and-compilation.md)
