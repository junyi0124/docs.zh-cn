---
title: 管理 XML 文档中的命名空间
description: 了解如何管理 XML 文档中的命名空间。 XML 命名空间将 XML 文档中的元素和属性名称与自定义和预定义的 URI 关联起来。
ms.date: 03/30/2017
ms.assetid: 682643fc-b848-4e42-8c0d-50deeaeb5f2a
ms.openlocfilehash: 120493de430c2372f3f71d1d1498ba880feda3d1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720147"
---
# <a name="managing-namespaces-in-an-xml-document"></a>管理 XML 文档中的命名空间

XML 命名空间将 XML 文档中的元素和属性名称与自定义和预定义的 URI 关联起来。 要创建这些关联，您应为命名空间 URI 定义前缀，并使用这些前缀来限定 XML 数据中的元素和属性名称。 命名空间可防止元素和属性名称冲突，并允许以不同方式处理和验证同名的元素和属性。  
  
<a name="declare"></a>

## <a name="declaring-namespaces"></a>声明命名空间  

 若要对某个元素声明命名空间，请使用 `xmlns:` 属性：  
  
 `xmlns:<name>=<"uri">`  
  
 其中，`<name>` 是命名空间前缀，`<"uri">` 是用于标识命名空间的 URI。 在声明前缀后，可使用该前缀来限定 XML 文档中的元素和属性，并将它们与命名空间 URI 相关联。 因为命名空间前缀在整个文档中使用，所以它的长度应较短。  
  
 本示例定义两个 `BOOK` 元素。 第一个元素由前缀 `mybook` 限定，第二个元素由前缀 `bb` 限定。 每个前缀都与不同的命名空间 URI 相关联：  
  
```xml  
<mybook:BOOK xmlns:mybook="http://www.contoso.com/books.dtd">  
<bb:BOOK xmlns:bb="urn:blueyonderairlines" />
</mybook>
```  
  
 若要指示某个元素是特定命名空间的一部分，请向其添加命名空间前缀。 例如，如果 `Author` 元素属于 `mybook` 命名空间，则将其声明为 `<mybook:Author>`。  
  
<a name="scope"></a>

## <a name="declaration-scope"></a>声明范围  

 命名空间在从声明点开始直至声明它的元素结束这一范围内有效。 在此示例中，在 `BOOK` 元素中定义的命名空间不适用于 `BOOK` 元素外部的元素（例如，`Publisher` 元素）：  
  
```xml  
<Author>Joe Smith</Author>  
<BOOK xmlns:book="http://www.contoso.com">  
    <title>My Wonderful Day</title>  
      <price>$3.95</price>  
</BOOK>  
<Publisher>  
    <Name>MSPress</Name>  
</Publisher>  
```  
  
 必须先声明命名空间，然后才可以使用它，但命名空间不一定出现在 XML 文档的顶部。  
  
 当您在 XML 文档中使用多个命名空间时，可以将一个命名空间定义为默认命名空间以产生外观更整洁的文档。 默认命名空间是在根元素中声明的，它适用于文档中的所有非限定元素。 默认命名空间只适用于元素，不适用于属性。  
  
 若要使用默认命名空间，请在该元素的声明中省略前缀和冒号：  
  
```xml  
<BOOK xmlns="http://www.contoso.com/books.dtd">  
...
</BOOK>
```  
  
## <a name="managing-namespaces"></a>管理命名空间  

 <xref:System.Xml.XmlNamespaceManager> 类存储命名空间 URI 及其前缀的集合，您可在此集合中查找、添加和删除命名空间。 在某些上下文中，需要使用此类以获得更佳 XML 处理性能。 例如，<xref:System.Xml.Xsl.XsltContext> 类使用 <xref:System.Xml.XmlNamespaceManager> 以获得 XPath 支持。  
  
 命名空间管理器对命名空间不执行任何验证，而是假定前缀和命名空间已经过验证并符合 [W3C 命名空间](https://www.w3.org/TR/REC-xml-names/)规范。  
  
> [!NOTE]
> [C#](../../linq/linq-xml-overview.md) 和 [Visual Basic](../../linq/linq-xml-overview.md) 中的 LINQ TO XML 不使用 <xref:System.Xml.XmlNamespaceManager> 来管理命名空间。 若要了解如何在使用 LINQ to XML 时管理命名空间，请参阅 LINQ 文档中的[使用 XML 命名空间 (C#)](../../linq/namespaces-overview.md) 和[使用 XML 命名空间 (Visual Basic)](../../linq/namespaces-overview.md)。  
  
 下面是可使用 <xref:System.Xml.XmlNamespaceManager> 类执行的一些管理和查找任务。 有关更多信息和示例，请遵循指向每个方法或属性的引用页的链接。  
  
|功能|使用|  
|--------|---------|  
|添加命名空间|<xref:System.Xml.XmlNamespaceManager.AddNamespace%2A> 方法|  
|移除命名空间|<xref:System.Xml.XmlNamespaceManager.RemoveNamespace%2A> 方法|  
|查找默认命名空间的 URI|<xref:System.Xml.XmlNamespaceManager.DefaultNamespace%2A> 属性|  
|查找命名空间前缀的 URI|<xref:System.Xml.XmlNamespaceManager.LookupNamespace%2A> 方法|  
|查找命名空间 URI 的前缀|<xref:System.Xml.XmlNamespaceManager.LookupPrefix%2A> 方法|  
|获取当前节点中命名空间的列表|<xref:System.Xml.XmlNamespaceManager.GetNamespacesInScope%2A> 方法|  
|限定命名空间的范围|<xref:System.Xml.XmlNamespaceManager.PushScope%2A> 和 <xref:System.Xml.XmlNamespaceManager.PopScope%2A> 方法|  
|检查是否在当前范围内定义了前缀|<xref:System.Xml.XmlNamespaceManager.HasNamespace%2A> 方法|  
|获取用于查找前缀和 URI 的名称表|<xref:System.Xml.XmlNamespaceManager.NameTable%2A> 属性|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlNamespaceManager>
- [XML 文档和数据](index.md)
