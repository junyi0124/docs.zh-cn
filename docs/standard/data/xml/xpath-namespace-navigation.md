---
title: XPath 命名空间浏览
ms.date: 03/30/2017
ms.assetid: 06cc7abb-7416-415c-9dd6-67751b8cabd5
ms.openlocfilehash: d5350f0c8f73c5efb0f7d9e4c492a841b4978c36
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720888"
---
# <a name="xpath-namespace-navigation"></a>XPath 命名空间浏览

要对 XML 文档使用 XPath 查询，必须正确定位 XML 命名空间以及命名空间中包含的元素。 命名空间可防止在多个上下文中使用名称时可能产生的混淆情况；例如，名称 `ID` 可能引用与 XML 文档的不同元素相关联的多个标识符。 命名空间语法指定了 URI、名称和前缀，可区分 XML 文档的各个元素。  
  
 本主题中的示例演示了通过 <xref:System.Xml.XPath.XPathNavigator> 在浏览 XML 文档时使用前缀。 有关命名空间和语法的详细信息，请参阅 [XML 文件：了解 XML 命名空间](/previous-versions/dotnet/articles/bb986013(v=msdn.10))。  
  
## <a name="namespace-declarations"></a>命名空间声明  

 命名空间声明使得在使用 <xref:System.Xml.XPath.XPathNavigator> 的实例时，很容易区分和定位 XML 文档的各个元素。 命名空间前缀提供了一种简化的语法，用来定位命名空间。  
  
 前缀由以下形式定义：`<e:Envelope xmlns:e=http://schemas.xmlsoap.org/soap/envelope/>.` 在此语法中，前缀“`e`”是命名空间的正式 URI 的缩写。 使用此语法可以将 `Body` 元素标识为 `Envelope` 命名空间的成员：`e:Body`。  
  
 在下一节的浏览示例中，下面的 XML 文档将用作 `response.xml`。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<e:Envelope xmlns:e="http://schemas.xmlsoap.org/soap/envelope/">  
  <e:Body>  
    <s:Search xmlns:s="http://schemas.microsoft.com/v1/Search">  
      <r:request xmlns:r="http://schemas.microsoft.com/v1/Search/metadata"
                 xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
      </r:request>  
    </s:Search>  
  </e:Body>  
</e:Envelope>  
```  
  
## <a name="navigation-by-namespace-prefix"></a>通过命名空间前缀进行浏览  

 本节中的代码使用 <xref:System.Xml.XPath.XPathNavigator> 和 <xref:System.Xml.XmlNamespaceManager> 对象，从前一节的 XML 文档中选择 `Search` 元素。 查询 `xpath` 对路径中的每个元素都包含了命名空间前缀。 指定包含每个元素的命名空间的精确标识，可确保通过 `Search` 方法正确浏览至 <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A> 元素。  
  
```csharp  
using (XmlReader reader = XmlReader.Create("response.xml"))  
{  
    XPathDocument doc = new XPathDocument(reader);  
    XPathNavigator nav = doc.CreateNavigator();
  
    XmlNamespaceManager nsmgr = new XmlNamespaceManager(nav.NameTable);  
    nsmgr.AddNamespace("e", @"http://schemas.xmlsoap.org/soap/envelope/");  
    nsmgr.AddNamespace("s", @"http://schemas.microsoft.com/v1/Search");  
    nsmgr.AddNamespace("r", @"http://schemas.microsoft.com/v1/Search/metadata");  
    nsmgr.AddNamespace("i", @"http://www.w3.org/2001/XMLSchema-instance");  
  
    string xpath = "/e:Envelope/e:Body/s:Search";  
  
    XPathNavigator element = nav.SelectSingleNode(xpath, nsmgr);  
  
    Console.WriteLine("Element Prefix:" + element.Prefix +
    " Local name:" + element.LocalName);  
    Console.WriteLine("Namespace URI: " + element.NamespaceURI);  
}  
```  
  
 完全限定的命名空间和名称的精确度不仅仅是一种方便。 对前面的示例中的文档定义和代码进行一项小实验，可以验证如果不使用完全限定的元素名称进行浏览，就会引发异常。 例如，元素定义为 `<Search xmlns="http://schemas.microsoft.com/v1/Search">`，而查询字符串 `xpath = "/s:Envelope/s:Body/Search";` 没有对 `Search` 元素使用命名空间前缀，则此查询将返回 `null`，而不是 `Search` 元素。  
  
## <a name="see-also"></a>请参阅

- [使用 XPathNavigator 访问 XML 数据](accessing-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 选择、计算和匹配 XML 数据](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)
