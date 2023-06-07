---
title: 命名空间的名称
description: 使用这些准则来命名命名空间，作为设计扩展和与 .NET 库交互的库的指南的一部分。
ms.date: 10/22/2008
helpviewer_keywords:
- names [.NET Framework], conflicts
- names [.NET Framework], namespaces
- type names, conflicts
- namespaces [.NET Framework], names
- names [.NET Framework], type names
ms.assetid: a49058d2-0276-43a7-9502-04adddf857b2
ms.openlocfilehash: 561a6e4ed1abf82fc1412123a4558a63e7176b2f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706484"
---
# <a name="names-of-namespaces"></a>命名空间的名称

与其他命名准则一样，命名命名空间的目标是为程序员创建足够的清晰度，使其能够立即了解命名空间的内容。 以下模板指定命名空间的一般规则：

 `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`

 下面是一些示例：

 `Fabrikam.Math` `Litware.Security`

 ✔️使用公司名称作为命名空间名称的前缀，以防不同公司的命名空间具有相同的名称。

 ✔️在命名空间名称的第二级使用稳定的独立于版本的产品名称。

 ❌ 不要使用组织层次结构作为命名空间层次结构中的名称的基础，因为公司内的组名通常是短期的。 围绕一组相关技术组织命名空间的层次结构。

 ✔️使用 PascalCasing，并使用句点分隔命名空间组件 (例如， `Microsoft.Office.PowerPoint`) 。 如果品牌采用 nontraditional 大小写，则应遵循品牌定义的大小写，即使它与常规命名空间大小写不符。

 ✔️考虑在适当的情况下使用复数命名空间名称。

 例如，请使用 `System.Collections` 而不是 `System.Collection`。 不过，品牌名称和首字母缩写是此规则的例外情况。 例如，请使用 `System.IO` 而不是 `System.IOs`。

 ❌ 命名空间和该命名空间中的类型不要使用相同的名称。

 例如，不要将 `Debug` 用作命名空间名称，并 `Debug` 在同一命名空间中提供名为的类。 一些编译器需要完全限定此类类型。

### <a name="namespaces-and-type-name-conflicts"></a>命名空间和类型名称冲突

 ❌ 不要引入泛型类型名称 `Element` ，例如、、 `Node` `Log` 和 `Message` 。

 很多情况下，这样做会导致在常见方案中发生类型名称冲突。 应限定泛型类型名称 (`FormElement` 、 `XmlNode` 、 `EventLog` `SoapMessage`) 。

 为避免不同类别的命名空间的类型名称冲突，有一些特定的准则。

- **应用程序模型命名空间**

     属于单个应用程序模型的命名空间经常一起使用，但几乎不能与其他应用程序模型的命名空间一起使用。 例如， <xref:System.Windows.Forms?displayProperty=nameWithType> 命名空间非常少与 <xref:System.Web.UI?displayProperty=nameWithType> 命名空间一起使用。 下面列出了众所周知的应用程序模型命名空间组：

     `System.Windows*` `System.Web.UI*`

     ❌ 不要为单个应用程序模型中的命名空间中的类型指定相同的名称。

     例如，不要将名为的类型添加 `Page` 到 <xref:System.Web.UI.Adapters?displayProperty=nameWithType> 命名空间，因为该 <xref:System.Web.UI?displayProperty=nameWithType> 命名空间已经包含一个名为的类型 `Page` 。

- **基础结构命名空间**

     此组包含在开发常见应用程序期间很少导入的命名空间。 例如， `.Design` 命名空间主要在开发编程工具时使用。 避免与这些命名空间中的类型冲突并不重要。

- **核心命名空间**

     核心命名空间包括所有 `System` 命名空间（不包括应用程序模型的命名空间和基础结构命名空间）。 核心命名空间包括、、、 `System` `System.IO` `System.Xml` 和 `System.Net` 。

     ❌ 不要提供会与核心命名空间中的任何类型冲突的类型名称。

     例如，永远不要将 `Stream` 用作类型名称。 它会与 <xref:System.IO.Stream?displayProperty=nameWithType> 非常常用的类型冲突。

- **技术命名空间组**

     此类别包括) 的前两个命名空间节点相同的所有命名空间 `(<Company>.<Technology>*` ，例如 `Microsoft.Build.Utilities` 和 `Microsoft.Build.Tasks` 。 属于单个技术的类型不会相互冲突，这一点非常重要。

     ❌ 不要分配会与单个技术中的其他类型冲突的类型名称。

     ❌ 请勿引入技术命名空间和应用程序模型命名空间中的类型之间的类型名称冲突 (除非该技术不打算用于应用程序模型) 。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [命名准则](naming-guidelines.md)
