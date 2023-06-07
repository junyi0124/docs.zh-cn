---
title: XSLT 安全注意事项
ms.date: 03/30/2017
ms.assetid: fea695be-617c-4977-9567-140e820436fc
ms.openlocfilehash: a8d077cf35da56795cc0b0c22e9bab26ce99f3a2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685196"
---
# <a name="xslt-security-considerations"></a>XSLT 安全注意事项

XSLT 语言具有一组丰富的功能，为您带来强大的功能和灵活性。 其中的许多功能尽管非常有用，但是也可能会被外部源利用。 为了安全地使用 XSLT，必须了解在使用 XSLT 时出现的安全问题类型以及可以用于缓解这些风险的基本策略。  
  
## <a name="xslt-extensions"></a>XSLT 扩展  

 两种常用的 XSLT 扩展是样式表脚本和扩展对象。 这些扩展允许 XSLT 处理器执行代码。  
  
- 扩展对象为 XSL 转换添加编程功能。  
  
- 脚本可以使用 `msxsl:script` 扩展元素嵌入样式表。  
  
### <a name="extension-objects"></a>扩展对象  

 扩展对象使用 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 方法添加。 支持扩展对象要求具有 FullTrust 权限集。 这样可以确保在执行扩展对象代码时，不会发生权限升级。 如果在没有 FullTrust 权限的情况下尝试调用 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 方法，将引发安全异常。  
  
### <a name="style-sheet-scripts"></a>样式表脚本  

 脚本可以使用 `msxsl:script` 扩展元素嵌入样式表。 脚本支持是 <xref:System.Xml.Xsl.XslCompiledTransform> 类上的一项可选功能，默认情况下禁用该功能。 通过将 <xref:System.Xml.Xsl.XsltSettings.EnableScript%2A?displayProperty=nameWithType> 属性设置为 `true` 并将 <xref:System.Xml.Xsl.XsltSettings> 对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 方法，可以启用脚本。  
  
#### <a name="guidelines"></a>准则  

 只有样式表来自可信的源时，才应启用脚本。 如果不能验证样式表的源，或者样式表不是来自可信的源，则为 XSLT 设置参数传入 `null`。  
  
## <a name="external-resources"></a>外部资源  

 如果处理器需要解析 URI 引用，XSLT 语言包括 `xsl:import`、`xsl:include` 或 `document()` 函数等功能。 <xref:System.Xml.XmlResolver> 类用于解析外部资源。 在下列两种情况下，可能需要解析外部资源：  
  
- 在编译样式表时，<xref:System.Xml.XmlResolver> 用于解析 `xsl:import` 和 `xsl:include`。  
  
- 在执行转换时，<xref:System.Xml.XmlResolver> 用于解析 `document()` 函数。  
  
    > [!NOTE]
    > 默认情况下，`document()` 类禁用 <xref:System.Xml.Xsl.XslCompiledTransform> 函数。 通过将 <xref:System.Xml.Xsl.XsltSettings.EnableDocumentFunction%2A?displayProperty=nameWithType> 属性设置为 `true` 并将 <xref:System.Xml.Xsl.XsltSettings> 对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 方法，可以启用此功能。  
  
 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 和 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法均包括接受 <xref:System.Xml.XmlResolver> 作为一个参数的重载。 如果未指定 <xref:System.Xml.XmlResolver>，将使用没有用户凭据的默认 <xref:System.Xml.XmlUrlResolver>。  
  
#### <a name="guidelines"></a>准则  

 只有样式表来自可信的源时，才应启用 `document()` 函数。  
  
 下表说明可能需要指定 <xref:System.Xml.XmlResolver> 对象的情况：  
  
- 如果 XSLT 进程需要访问要求进行身份验证的网络资源，可以使用具有必要的凭据的 <xref:System.Xml.XmlResolver>。  
  
- 如果要限制 XSLT 进程可以访问的资源，可以使用具有正确权限集的 <xref:System.Xml.XmlSecureResolver>。 如果需要打开自己无法控制的或不可信的资源，请使用 <xref:System.Xml.XmlSecureResolver> 类。  
  
- 如果要自定义行为，可以实现自己的 <xref:System.Xml.XmlResolver> 类并使用该类解析资源。  
  
- 如果要确保不访问任何外部资源，可以为 `null` 自变量指定 <xref:System.Xml.XmlResolver>。  
  
## <a name="see-also"></a>请参阅

- [XSLT 转换](xslt-transformations.md)
- [在 XSLT 处理期间解析外部资源](resolving-external-resources-during-xslt-processing.md)
- [代码访问安全性](../../../framework/misc/code-access-security.md)
