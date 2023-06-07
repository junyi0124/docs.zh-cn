---
title: 抽象（抽象类型和接口）
ms.date: 10/22/2008
helpviewer_keywords:
- interfaces [.NET Framework], abstract
- abstract interfaces [.NET Framework]
- abstract types [.NET Framework]
- types [.NET Framework], abstract
ms.assetid: 0a632bc7-9b03-44ee-8842-c82f88672a45
ms.openlocfilehash: f5169cf730dc987526765c9538978901d424814b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701401"
---
# <a name="abstractions-abstract-types-and-interfaces"></a>抽象（抽象类型和接口）

抽象是一种类型，它描述协定，但不提供协定的完全实现。 抽象通常作为抽象类或接口实现，它们附带一组明确定义的参考文档，其中描述了实现协定的类型所需的语义。 .NET Framework 中的一些最重要的抽象包括 <xref:System.IO.Stream> 、 <xref:System.Collections.Generic.IEnumerable%601> 和 <xref:System.Object> 。

 您可以通过实现支持抽象协定的具体类型，并将此具体类型与) 抽象 (操作的框架 Api 结合使用来扩展框架。

 很难设计一种有意义且有用的抽象方法，它能够经受时间测试。 主要难点是获得正确的成员集，而不是更少的成员。 如果抽象的成员太多，就会变得难以甚至不可能实现。 如果它的成员对于承诺的功能而言太少，则在很多有趣的情况下它将变得毫无用处。

 框架中的抽象太多也会对框架的可用性产生负面影响。 很难理解抽象，而不了解它如何适应具体实现和在抽象上操作的 Api 的更大的了解。 此外，抽象和其成员的名称都必须是抽象的，这通常会使它们变得难懂和 unapproachable，而无需事先了解其使用情况的更广泛的上下文。

 不过，抽象提供了极其强大的可扩展性，其他扩展性机制通常不会匹配。 它们是多个体系结构模式的核心，如插件、控制反转 (IoC) 、管道等。 它们对于框架的可测试性也极其重要。 好的抽象使你可以出于单元测试的目的，为大量依赖关系提供存根。 总的来说，抽象负责面向面向对象的现代框架的丰富丰富。

 ❌ 不要提供抽象，除非通过开发一些具体的实现和使用抽象的 Api 来对它们进行测试。

 ✔️在设计抽象类和接口时，请仔细选择抽象类和接口。

 ✔️考虑为抽象的具体实现提供引用测试。 此类测试应该允许用户测试其实现是否能正确实现协定。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [扩展性设计](designing-for-extensibility.md)
