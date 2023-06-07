---
title: System.Xml 使用情况
ms.date: 10/22/2008
ms.assetid: 82302f0d-a621-4c6f-b57d-999bd61f21a6
ms.openlocfilehash: 46282afa6548c731b04c40d8de91a1fed997c57c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677565"
---
# <a name="systemxml-usage"></a>System.Xml 使用情况

本部分介绍如何使用 <xref:System.Xml?displayProperty=nameWithType> 可用于表示 XML 数据的命名空间中的多个类型。

 ❌ 不要使用 <xref:System.Xml.XmlNode> 或 <xref:System.Xml.XmlDocument> 来表示 XML 数据。 优选 <xref:System.Xml.XPath.IXPathNavigable> 改用的、 <xref:System.Xml.XmlReader> 、 <xref:System.Xml.XmlWriter> 或子类型 <xref:System.Xml.Linq.XNode> 的实例。 `XmlNode` 和 `XmlDocument` 不用于公开公共 api。

 ✔️使用 `XmlReader` 、 `IXPathNavigable` 或子类型 `XNode` 作为接受或返回 XML 的成员的输入或输出。

 使用这些抽象，而不是 `XmlDocument` 、 `XmlNode` 或 <xref:System.Xml.XPath.XPathDocument> ，因为这会使方法与内存中 XML 文档的特定实现分离，并允许它们使用公开、或的虚拟 XML 数据 `XNode` 源 `XmlReader` <xref:System.Xml.XPath.XPathNavigator> 。

 ❌`XmlDocument`如果要创建表示基础对象模型或数据源的 XML 视图的类型，请不要创建子类。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [使用准则](usage-guidelines.md)
